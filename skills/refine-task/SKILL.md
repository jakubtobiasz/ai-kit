---
name: refine-task
description: >-
  Synthesize a dev-ready TASK.md from a raw ticket (TICKET.md), a scout map
  of the affected codebase, and any resolved architect decisions. Ground
  intent, affected areas, approach, acceptance criteria, and test surface in
  real paths and patterns. Do not transcribe the ticket. Use once a ticket
  (with recon, or trivial enough to skip it) needs to become an
  implementation-ready TASK.md. Escalate via a NEEDS: scout / NEEDS: architect
  block when a fact or design decision is missing. Do not guess. Do NOT use
  to explore the codebase yourself, make a design decision, split an approved
  TASK.md into a PLAN.md, or write ticket bodies from scratch.
argument-hint: "[path to TICKET.md]"
aibits:
  deps:
    - ~/skills/asd-ste100
---

# Refine Task

## Purpose

Convert a raw ticket into a `TASK.md` that a developer can execute cold.

Synthesize. Do not transcribe. The ticket states what is wanted. This procedure bases that want on the real codebase. This procedure restates it in implementation terms.

## Activation

### Use when

Use this skill when:

- a ticket is ready to become an implementation-ready `TASK.md`
- recon exists, or the ticket is trivial enough to skip recon
- a caller asks to refine a task, write `TASK.md`, or ground a ticket in the codebase

### Do not use when

Do not use this skill when:

- the task is to explore the codebase (that is scout)
- the task is to make a design decision (that is architect)
- the task is to split an approved `TASK.md` into a `PLAN.md` (that is `split-task-into-plan`)
- the task is to write ticket bodies from scratch
- no ticket path can be resolved

## Context

Usual files live under `.ai/task/<task-id>/`. Write `TASK.md` there unless the caller names another target.

**Ground** means write paths and patterns that exist in the scout map. Do not invent them.

Assume no stack. Read the consuming repo's `CLAUDE.md` and `AGENTS.md`. Read the scout map.

This skill runs inside the refining agent. Do not spawn a Task. Do not select a model.

When acceptance criteria use Gherkin, apply `writing-gherkin-scenarios` if that skill is available.

## Workflow

1. Resolve the ticket path, the target `TASK.md` path, and the template path.
2. Read `assets/task-template.md`. Follow its structure exactly.
3. Read the ticket in full.
4. Read the scout map. The map may be empty for a trivial ticket.
5. Read architect decisions. They may be empty.
6. Read the consuming repo's `CLAUDE.md` and `AGENTS.md` when the scout map does not cover a convention.
7. Synthesize every template section. Do not copy ticket prose.
8. If a required fact or design call is missing, stop. Return the matching escalation. Do not guess.
9. Write one `TASK.md` at the target path. Fill every section with real content.
10. Run the quality checks. Then return.

### Synthesize. Do not transcribe

Reframe the ticket section by section.

- **Intent** — restate the ticket's want as what is actually being built, in implementation terms.
- **Affected areas** — real paths and roles. Take them only from the scout map.
- **Approach** — include architect decisions inline. If there is no design fork, write a plain approach from the scout map's existing patterns.
- **Acceptance criteria** — concrete Given/When/Then scenarios. Rewrite them against the now-grounded paths and behavior. Do not copy them verbatim from the ticket.
- **Test surface** — named test paths and what each covers. Take them from the scout map's own test conventions.
- **Out of scope** — keep the ticket's exclusions. Make them more precise where grounding reveals a boundary the ticket left implicit.

### Fill every section

Follow `assets/task-template.md`. Every section holds real content.

Leave none as a placeholder. Leave none as an empty stub. Do not restate the template's instructional comment as if it were content.

If you cannot fill a section, escalate. Do not leave it thin.

## Decision Rules

- If the ticket path cannot be resolved → stop. Name the required input. Do not guess.
- If "Affected areas" cannot be filled because a path is missing → return `NEEDS: scout`. Do not invent a path.
- If "Approach" needs a design fork the ticket does not settle → return `NEEDS: architect`. Do not pick a design.
- If the scout map is empty and the ticket is trivial → fill from the ticket plus `CLAUDE.md` / `AGENTS.md`. Escalate if a real path is still missing.
- If architect decisions are empty and there is no design fork → write a plain approach from existing patterns.
- If Gherkin is present and `writing-gherkin-scenarios` is available → apply that bar.
- If a section would be a placeholder → escalate. Do not write the placeholder.
- If the caller asks you to explore the repo to fill a gap → escalate to scout. Do not explore.

## Escalation contract

If you cannot fill "Affected areas" or "Approach" because a fact or a design decision is missing, stop. Return an escalation. Do not guess:

```
NEEDS: scout
<the recon question>
```

— or —

```
NEEDS: architect
<the design question + context>
```

A stalled task is better than a guessed path or an invented design call.

## Constraints

- MUST write only the target `TASK.md`.
- MUST fill every template section with real content.
- MUST take file paths only from the scout map.
- MUST reframe the ticket. MUST NOT copy its prose.
- MUST return a `NEEDS: scout` or `NEEDS: architect` block in the shape above when blocked.
- MUST NOT invent a file path that is not in the scout map.
- MUST NOT leave a template section as a placeholder.
- MUST NOT explore the codebase to fill a gap.
- MUST NOT make a design decision.
- MUST NOT spawn a Task subagent.
- MUST NOT select or hardcode a model.
- NEVER commit, push, or touch git. The orchestrator owns that.

## Output format

Write one `TASK.md` at the target path. Match `assets/task-template.md`.

If blocked, return only the escalation block. Do not write a thin `TASK.md`.

## Quality Checks

Before you finish:

- [ ] The ticket, the scout map, and any architect decisions were read.
- [ ] Every template section has real content. None is a placeholder.
- [ ] Intent is restated in implementation terms. It is not copied ticket prose.
- [ ] Every path in Affected areas exists in the scout map.
- [ ] Approach includes architect decisions, or a plain pattern-based approach when there is no fork.
- [ ] Acceptance criteria are concrete Given/When/Then against grounded paths.
- [ ] Test surface names real test paths from the scout map.
- [ ] Out of scope keeps the ticket's exclusions and any newly precise boundary.
- [ ] No file other than the target `TASK.md` was written.
- [ ] Git was not touched.

## Examples

### Missing path

Input: the ticket names a module the scout map does not list.

Expected: stop. Return `NEEDS: scout` with the recon question. Do not invent a path. Do not write `TASK.md`.

### Design fork

Input: two valid approaches exist. No architect decision is attached.

Expected: stop. Return `NEEDS: architect` with the design question and context. Do not pick one.

### Transcription

Input: the ticket's Intent paragraph is clear.

Expected: restate it in implementation terms. Do not paste the ticket paragraph into `TASK.md`.

### Trivial ticket

Input: the scout map is empty. The ticket touches one obvious file named in `AGENTS.md`.

Expected: fill Affected areas from that named file. Escalate if no real path can be shown.

## Failure Modes

- Ticket path missing and not inferable → stop. Name the required input. Do not guess.
- Scout map empty and a real path is still unknown → `NEEDS: scout`. Do not invent paths.
- Design fork with no architect decision → `NEEDS: architect`. Do not pick a design.
- Template section cannot be filled → escalate. Do not write a placeholder.
- Caller asks you to search the repo → escalate to scout. Do not explore.

## References

- `assets/task-template.md` — `TASK.md` structure. Read once per run. Follow it exactly.
- `writing-gherkin-scenarios` — Gherkin AC bar. Read when AC is Gherkin and the skill is available.
- `split-task-into-plan` — next phase after `TASK.md` is approved. Do not run it here.
