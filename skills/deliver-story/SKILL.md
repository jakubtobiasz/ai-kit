---
name: deliver-story
description: >-
  Deliver a whole approved story or epic. Deliver every ticket of a
  .ai/idea/<slug>/ breakdown as reviewed, resumable implementations. Run those
  implementations in dependency waves. Use this skill when the user wants the
  entire breakdown built rather than one ticket. Triggers include "deliver this
  story", "run the whole breakdown", "execute the epic", "build every ticket",
  "deliver .ai/idea/<slug>". Read the parent (STORY.md/EPIC.md) and each
  TASK_NN.md. Build a wave graph from the Depends on fields. Abort loudly on a
  cycle before any state is written. Ask one story mode up front. Gated mode
  keeps every per-ticket gate live. Gated mode runs tickets strictly
  sequentially. Autonomous mode waives gates. Autonomous mode allows
  within-wave file-disjoint parallelism. Then run each ticket through the same
  Refine, Split, Implement, and Review dispatch that develop uses. Resume from
  per-ticket .ai/task/<id>/ PLAN states. Do NOT use for a single ticket (that
  is develop). Do NOT use to draft/scope tickets from scratch (that is the
  delivery-planner flow).
argument-hint: "[path to .ai/idea/<slug>/ or its STORY.md / EPIC.md]"
license: MIT
metadata:
  version: "1.0.0"
aibits:
  deps:
    - ~/skills/develop
---

# Deliver Story

## Purpose

Deliver a whole `.ai/idea/<slug>/` breakdown. Run its tickets in dependency waves. You are the orchestrator. You run in the main thread. That thread is the only place that can spawn subagents.

You own the wave graph, the story mode, every gate, and every commit. You never write `TASK.md`, `PLAN.md`, or application code yourself.

`develop` is not a callable subroutine. `develop` is a main-thread skill. One main-thread skill cannot invoke another as a function. The per-ticket dispatch and gate logic below is a deliberate, limited restatement of develop's mechanics. It is not a call into `develop`.

## Activation

### Use when

Use this skill when:

- the user wants every ticket in an approved `.ai/idea/<slug>/` breakdown built
- the input is `.ai/idea/<slug>/` or its `STORY.md` / `EPIC.md`
- work must resume from per-ticket `.ai/task/<id>/` PLAN states

### Do not use when

Do not use this skill when:

- the user wants a single ticket built (`develop`)
- the user wants tickets drafted or scoped from scratch (`delivery-planner`)
- the user wants PR or code-review follow-ups on one existing task (`address-review`)

## Context

Each ticket runs the same phase dispatches as `develop`. Use **developer** for Refine (`refine-task`), Split (`split-task-into-plan`), and Implement (`implement-group`). Use the **reviewer** role for Review (`review-implementation`) via Task `generalPurpose` only.

**Iron rule.** Subagents cannot spawn subagents. When a dispatched agent returns a block that starts `NEEDS: scout` or `NEEDS: architect` (or `NEEDS: senior architect`), YOU spawn that agent. Then re-dispatch the original agent with the answer appended to its prompt. The orchestrator always handles escalation.

**Reviewer dispatch.** NEVER use `subagent_type: reviewer`. That type is force-ask-mode and blocks Shell. The failure is "Shell unavailable". Never use `subagent_type: reviewer`. Dispatch the reviewer role as `generalPurpose`. Prompt: Read the reviewer agent (`agents/reviewer.md` or `.cursor/agents/reviewer.md`). Then follow `skills/review-implementation/SKILL.md`.

**Model dispatch.** Always pass Task `model`. Follow the consuming repo's model-picking rule when it exists.

- If the consuming repo has a model-picking rule, that rule wins.
- Else use the model the user named this turn.
- Else inherit only if the parent is already the intended family. Disclose the inherit.
- Never omit `model`. Never silently swap families.
- Senior role: same agent type. Use the stronger model the consuming repo names for Senior. Do not invent a second agent file.
- Scout and cheap recon: use the cheaper model the consuming repo names for recon.
- Reviewer role: `generalPurpose` only.

Each dispatch passes the assigned skill's file path plus that skill's declared inputs. The agent Reads that `SKILL.md` and follows it to the end.

**Project context is read, not assumed.** This skill assumes no stack. Each ticket's scout recon reads the target repo's own `CLAUDE.md` / `AGENTS.md`. Every downstream agent inherits project context from there. This skill works in any repository.

## Workflow

```mermaid
flowchart TD
    Start([/deliver-story]) --> Resolve["Read parent + every TASK_NN.md — §1"]
    Resolve --> Cycle{"Dependency cycle? — §1"}
    Cycle -->|yes| Abort([Abort: name the cycle, no .ai/task/ state written])
    Cycle -->|no| Resume{"Existing .ai/task/ state? — §0"}
    Resume -->|resume| Rebuild["Rebuild parent cache from PLAN states — §0"]
    Resume -->|fresh| Mode["Ask story mode once — §2"]
    Rebuild --> Mode
    Mode --> NextWave{"Next runnable wave? — §3"}
    NextWave -->|none left| Done([Story delivered — resumable])
    NextWave -->|wave ready| Wave["Execute wave — §3"]
    Wave --> Ticket["Per-ticket pipeline per lane — §4"]
    Ticket --> Gate{"Blocked lane? — §5"}
    Gate -->|yes| Hold["Hold its dependents, continue independent lanes — §5"]
    Gate -->|no| Progress["Suffix parent task list — §6"]
    Hold --> Progress
    Progress --> NextWave
```

The numbered steps are the authority.

### 0. Resume

`.ai/task/<task-id>/PLAN.md` states are **authoritative**. The parent task list is a derived cache. On entry, before you act, reconcile the two. Tell the user where you resume.

1. For each ticket, read its `.ai/task/<task-id>/`.
2. A ticket whose every `PLAN.md` group is `[x] committed` (or a legacy plan whose groups are plain `[x] done`) is **delivered**. Skip it.
3. A ticket with groups in progress resumes mid-pipeline per develop's own §0. Implemented-but-unreviewed groups route through Review before any commit.
4. If the parent list's suffixes disagree with the PLAN states, the PLAN states win. Rewrite the parent suffixes to match (§6). Do not restructure the parent template.
5. Then continue at the next runnable wave (§3). A ticket with no state dir yet has not started.

### 1. Read the breakdown and build the wave graph

1. **Resolve the parent.** The input is `.ai/idea/<slug>/` (or its `STORY.md` / `EPIC.md`). Read the parent and every `TASK_NN.md` child in the folder.
2. **Derive each ticket's task-id** exactly as develop does for an idea file. Parse the ticket's trailing `<!-- Tracker key: ... -->` comment. A recorded real key wins. Otherwise `<slug>-task-NN`, with `NN` copied as written (two digits) from the filename. Never invent a tracker key.
3. **Build the dependency graph** from each child's `- **Depends on:**` field. `none` = a root. Otherwise a comma-separated list of `TASK_NN` tokens, each an edge from the named ticket. Wave 0 = every ticket with no unmet dependency. Wave N = every ticket whose dependencies all are in earlier waves.
4. **Cycle check FIRST.** If the edges do not form a DAG (some tickets never become runnable), **abort**. Name the offending tickets in the cycle. Do this **before** creating any `.ai/task/` state. A cycle-aborted run writes nothing.

Create state only after the graph is clean.

### 2. Story mode — asked once, up front

Ask via `AskUserQuestion`, once per invocation, before any wave runs. This is the **only** place per-ticket gates may be waived. Waive gates only by the user's explicit choice.

- **gated** (default) — every per-ticket TASK/PLAN/commit gate stays live. Tickets run **strictly sequentially** in dependency order. Waves only fix that order. No within-wave parallelism.
- **autonomous** — the user explicitly waives the per-ticket **TASK** and **PLAN** approval gates for the whole story. In the same prompt the user picks the story-wide **commit-gate mode**. The choices are auto-commit each group, or review-each. Review-each: present each group's diff and wait. Within-wave parallelism is allowed (§3).

The per-ticket mode questions develop would ask (fast-path confirmation, commit-gate mode) are **suppressed**. The story mode owns them for the whole run.

### 3. Wave execution

Run waves in order. **Wave N+1 starts only when every ticket its members depend on is delivered and reviewed.** Delivered and reviewed means all their groups are committed. Committed groups require that review has passed.

**Gated mode.** Run the wave's tickets one at a time in dependency order. Run each ticket through the full per-ticket pipeline (§4) with every gate live. Waves only constrain ordering. There is no parallelism.

**Autonomous mode — two passes.** Disjointness is unknowable at wave-planning time. A ticket's `Files:` lists exist only after Split.

1. **Pass A — Refine + Split every ticket in the wave.** These dispatches write only under `.ai/task/<id>/`. They are conflict-free. They may run in parallel.
2. **Pass B — Implement + Review.** Read the `Files:` lists from the produced PLANs. Tickets whose PLANs share no file **implement in parallel**. Tickets whose PLANs touch a common file **serialize** within the wave (the working tree is shared). Each ticket runs its Implement, Review, and commit loop (§4).

### 4. Per-ticket pipeline

Each ticket runs the same four phase dispatches as develop. The story mode (§2) sets the gate behavior. A per-ticket question does not set the gate behavior. Handle every `NEEDS:` escalation here per the iron rule. Re-dispatch until the phase returns.

Do not invoke `develop` as a function. Restate the loop here.

1. **Write TICKET.md.** Create `.ai/task/<task-id>/`. Write the child `TASK_NN.md` verbatim to `TICKET.md`. The idea file's tracker-key line stays authoritative.
2. **Refine.** Recon on demand (spawn **scout**, and **architect** for a real design fork). Then spawn **developer** (pass Task `model`; consuming-repo picker wins) with the skill `skills/refine-task/SKILL.md`, and its inputs, to write `TASK.md`. **Gated:** present `TASK.md` for approval. Loop until approved. **Autonomous:** the gate is waived. Proceed.
3. **Split.** Spawn **developer** (pass Task `model`; consuming-repo picker wins) with the skill `skills/split-task-into-plan/SKILL.md`, and its inputs, to write `PLAN.md`. **Gated:** present `PLAN.md` (graph, group titles, per-group commit messages, dependencies). Loop until approved. **Autonomous:** the gate is waived. Proceed. After approval, sync `PLAN.md` onto the tracker issue when the ticket has a real tracker key. Use the same procedure as develop §7.
4. **Implement + Review** per group, following develop's §4/§5 loop. Spawn **developer** (pass Task `model`; consuming-repo picker wins) with `skills/implement-group/SKILL.md`. On **DONE**, set `[x] implemented`. Then spawn the **reviewer** role via Task `generalPurpose`. Do not use `subagent_type: reviewer`. Take the pass Task `model`; consuming-repo picker wins. Prompt: Read the reviewer agent (`agents/reviewer.md` or `.cursor/agents/reviewer.md`). Then follow `skills/review-implementation/SKILL.md` over the group's diff. Loop findings back to the developer. Pass accepted findings as its optional findings input. Continue until the review is clean or waived. Then set `[x] reviewed`. Then commit. **Commit gate:** gated mode presents each group's diff and waits (review-each). Autonomous mode uses the story-wide mode chosen in §2. You own every commit. Set `[x] committed`. After every `PLAN.md` progress write, sync the tracker PLAN attachment again (develop §7). Other agents then see live checkboxes on the issue.
5. On a group **BLOCKED** return, leave it unchecked. Handle it per §5.

### 5. Wave gating and blocked-lane isolation

- A ticket is **done** for gating purposes only when all its PLAN groups are `[x] committed`. The next wave waits for that.
- A **BLOCKED** ticket stops **only its own dependents' lane**. It never stops the whole story. Show the block (which ticket, why). Hold every ticket that transitively depends on it. **Continue every independent lane** in the current and subsequent waves. A later invocation can resume the blocked lane once unblocked.

### 6. Progress tracking

- After a ticket is delivered, append a status suffix to **its** entry in the parent's numbered task list — for example `1. [TASK_00](TASK_00.md) — Create refine-task skill — delivered (<task-id>)`. Use `— in progress`, `— delivered (<task-id>)`, or `— BLOCKED (<reason>)`. An untouched ticket keeps no suffix. Edit only the suffix. **Do not restructure the parent template**.
- The parent list is a derived cache of the authoritative per-ticket `PLAN.md` states. Whenever the two disagree (notably on resume, §0), rebuild the suffixes from the PLAN states.
- **Resumable.** A later invocation reads the graph again. It skips delivered tickets. It starts the next runnable wave.

## Decision Rules

- If the dependency edges are not a DAG → abort. Name the cycle. Write no `.ai/task/` state.
- If PLAN states disagree with parent suffixes → PLAN states win. Rewrite suffixes only.
- If every group on a ticket is `[x] committed` (or legacy `[x] done`) → skip that ticket.
- If a ticket has `[x] implemented` without `[x] reviewed` → Review before any commit.
- If story mode is gated → sequential tickets. Every TASK/PLAN/commit gate stays live. No within-wave parallelism.
- If story mode is autonomous → TASK and PLAN gates are waived. Commit-gate mode is the story-wide choice from §2.
- If autonomous Pass A → Refine + Split may run in parallel. Those writes stay under `.ai/task/<id>/`.
- If autonomous Pass B and PLANs share a file → serialize those tickets. Else implement in parallel.
- If a ticket is BLOCKED → hold its dependents. Continue independent lanes.
- If a dispatched agent returns `NEEDS: scout` or `NEEDS: architect` → you spawn that agent. Then re-dispatch.
- If `NEEDS: senior <role>` → same agent type. Stronger model per the consuming repo.
- If you are tempted to call `develop` as a function → do not. Restate the per-ticket loop here.

## Constraints

- MUST run in the main thread.
- MUST NOT invoke `develop` as a function.
- MUST pass Task `model` on every spawn. MUST follow the consuming repo's model-picking rule when it exists.
- MUST dispatch the reviewer role as `generalPurpose`. MUST NOT use `subagent_type: reviewer`.
- MUST handle every `NEEDS:` escalation yourself.
- MUST abort on a dependency cycle before any `.ai/task/` state is written.
- MUST NOT invent a tracker key.
- MUST NOT write `TASK.md`, `PLAN.md`, or application code yourself. Dispatch instead.
- MUST NOT restructure the parent template. Edit only status suffixes.
- MUST NOT waive per-ticket gates except by the user's explicit story-mode choice.
- MUST NOT stop the whole story because one lane is BLOCKED.
- MUST treat per-ticket `PLAN.md` as authoritative over the parent list.
- NEVER silently swap model families.
- NEVER omit Task `model`.

## Quality Checks

Before you finish:

- [ ] Parent and every `TASK_NN.md` were read. Wave graph was built from `Depends on`.
- [ ] Cycle check ran before any `.ai/task/` write.
- [ ] Story mode was asked once up front.
- [ ] `develop` was not invoked as a function.
- [ ] Each ticket used refine-task, split-task-into-plan, implement-group, and review-implementation.
- [ ] Reviewer ran as `generalPurpose`. Model was passed.
- [ ] Every `NEEDS:` escalation was fulfilled by the orchestrator.
- [ ] Resume used per-ticket PLAN three-state Status.
- [ ] Parent suffixes match PLAN states. The parent template was not restructured.
- [ ] BLOCKED lanes held dependents only. Independent lanes continued.

## Examples

### Fresh breakdown, gated

Input: `deliver .ai/idea/checkout/` with three tickets, `TASK_01` depends on `TASK_00`.

Expected: cycle check passes. Ask story mode. User picks gated. Run `TASK_00` through the full pipeline with live gates. Then `TASK_01`. Then `TASK_02` if it is a root or its deps are delivered. Suffix the parent list. Do not call `develop`.

### Cycle

Input: `TASK_00` depends on `TASK_01`. `TASK_01` depends on `TASK_00`.

Expected: abort. Name both tickets. Write no `.ai/task/` state.

### BLOCKED lane

Input: wave 0 ticket `TASK_00` returns BLOCKED. `TASK_02` does not depend on it.

Expected: show which ticket and why. Hold `TASK_00` dependents. Continue `TASK_02`. Do not stop the story.

### Resume

Input: `/deliver-story .ai/idea/checkout/` and `TASK_00` groups are `[x] committed`. `TASK_01` has `[x] implemented` without `[x] reviewed`.

Expected: skip `TASK_00`. Route `TASK_01` through Review before commit. Rebuild parent suffixes from PLAN states.

### Wrong skill

Input: "implement TICKET-123".

Expected: refuse this skill. Point to `develop`.

## Failure Modes

- Parent path missing → ask once for `.ai/idea/<slug>/` or its `STORY.md` / `EPIC.md`. Do not guess a slug.
- Dependency cycle → abort. Name the tickets. Write nothing.
- Tracker key comment is the placeholder → use `<slug>-task-NN`. Do not invent a key.
- Reviewer dispatched as `subagent_type: reviewer` → "Shell unavailable". Re-dispatch as `generalPurpose`.
- One ticket BLOCKED → isolate that lane. Continue independent lanes.
- Parent suffixes drift from PLAN states → rebuild suffixes. Do not restart delivered tickets.
- Temptation to call `develop` → restate §4 here instead.

## References

- `skills/develop/SKILL.md` — per-ticket loop and tracker PLAN sync (§4, §5, §7). Read as the restatement source. Do not invoke it.
- `skills/refine-task/SKILL.md` — Refine. Pass this path to developer.
- `skills/split-task-into-plan/SKILL.md` — Split. Pass this path to developer.
- `skills/implement-group/SKILL.md` — Implement. Pass this path to developer.
- `skills/review-implementation/SKILL.md` — Review. Pass this path to the reviewer role.
- `agents/reviewer.md` — reviewer role. Read from `.cursor/agents/reviewer.md` when that copy is the runtime file.
- `address-review` — PR feedback rounds on one existing task.
