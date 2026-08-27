---
name: review-implementation
description: >-
  Run a fixed, findings-only checklist over an implemented PLAN.md group's
  diff or a whole task's diff. Include its TASK.md/PLAN.md context. Run it
  before the group is committed. Use when someone asks "review this
  implementation", "review group G1", "check this diff before commit", "did
  this actually meet the AC", "is this ready to commit", or when an implement
  phase runs its review gate. Run six fixed checks. Trace acceptance criteria
  to the change. Check that scope matches the group's Files. Check that tests
  are real and green. Check that repo conventions are followed (from the
  consuming repo's own CLAUDE.md / scout map, never a hardcoded style). Check
  that there are no stray artifacts. Check that there are no silent failure
  paths. Return severity-ranked findings that cite file and line. Never edit
  a reviewed file. The caller re-dispatches any fix. Do NOT use this to
  review a ticket, TASK.md, or plan before implementation starts. This skill
  only reviews code and tests that have already been written.
argument-hint: "[task-id [group-id]]"
aibits:
  deps:
    - ~/skills/asd-ste100
---

# Review Implementation

## Purpose

This is a routine gate, not an investigation. Run the same six checks every time over a diff that claims to be done.

Catch a group that skipped an AC, drifted out of scope, or shipped on red tests. Catch it before it is committed. Do not catch it after.

Read the diff plus its `TASK.md` / `PLAN.md` context. Report findings. Never rewrite, fix, or touch the reviewed files.

## Activation

### Use when

Use this skill when:

- a `PLAN.md` group is implemented and needs a review before commit
- a caller asks to review this implementation, review group G1, check this diff before commit, or ask if the AC is met
- an implement phase runs its review gate
- optionally, every group is implemented and a whole-task sweep is requested

**Per-group review is the commit gate.** Use one invocation per group.

**A whole-task sweep is optional.** It is an extra pass. It is not part of the commit path. Nothing downstream depends on it running.

### Do not use when

Do not use this skill when:

- the target is a ticket, `TASK.md`, or plan before implementation starts
- the task is to edit, fix, or rewrite the reviewed files
- the task is to implement the group (that is `implement-group`)
- the task is to triage review comments (that is `triage-review-feedback`)

## Context

Usual files live under `.ai/task/<task-id>/`. Per-group review uses that group's block in `PLAN.md` or `review/R<n>/POST_REVIEW_PLAN.md`.

Assume no stack. Read the consuming repo's `CLAUDE.md` and `AGENTS.md`. Read the scout map if one was already produced earlier in this task (`.ai/task/<id>/SCOUT.md`). Never assume conventions from outside this repo.

This skill runs inside the reviewing agent. Do not spawn a Task. Do not select a model.

Callers that dispatch this skill must always pass a model. Never use `subagent_type: reviewer`. That type is force-ask-mode and blocks Bash. The failure is "Shell unavailable". The consuming repo policy wins. Do not hardcode a model family.

If Shell is unavailable this session, report check 3 as a blocker. Tell the caller to re-dispatch without `subagent_type: reviewer`. Do not treat the gate as green.

## Input

- **Per-group (the commit gate):** Use the group's diff. The diff is the working-tree changes for the group's `Files:` and their tests. Also use the group's own block in `PLAN.md` or `POST_REVIEW_PLAN.md` (`Files:`, `Steps:`, `Status:`, commit message). Also use the acceptance slice. For a develop group, the slice is the part of `TASK.md`'s `## Acceptance criteria` that this block serves. For post-review, the slice is the actionable asks of the feedback ids listed in the group's `Addresses:`.
- **Whole-task sweep (optional):** Use the combined diff across every implemented group in `.ai/task/<task-id>/`. Also use the full `TASK.md` and `PLAN.md`.
- Either way: the consuming repo's own `CLAUDE.md` / `AGENTS.md` (and a scout map, if one was already produced earlier in this task) for check 4.

## Workflow

1. **Read the target.** Read the diff. Read the group's `PLAN.md` / `POST_REVIEW_PLAN.md` block. For a sweep, read the whole plan. Read the acceptance slice (`TASK.md` AC or addressed feedback asks).
2. **Run all six checks below, in order, against the diff.** Do not skip a check because an earlier one failed. A group can fail scope and still be checked for stray artifacts.
3. **For check 3, actually run the tests.** Do not trust a `DONE` report. Run the project's own test command (from the repo's `CLAUDE.md` / task runner). Read the real output. This requires a working Shell.

   If Shell is unavailable this session, report check 3 as a blocker. Tell the caller to re-dispatch without `subagent_type: reviewer`. Do not treat the gate as green.

4. **Report findings.** Rank them most-severe first. Cite file and line for each. Do not edit any file. Report findings only. The caller dispatches the fix. The caller then re-reviews.

## The checklist

### 1. Acceptance criteria met

**Passing:** every acceptance item for the group maps to a concrete piece of the diff. For a develop group, that item is each Gherkin `Scenario` in the group's slice of `TASK.md`. For a post-review group, that item is each actionable ask of the feedback ids in `Addresses:`. The described outcome must be a real code path in the change.

**Fail citing:** quote the scenario. Then give the file:line where the behavior is missing. Or cite the file:line that does something other than what the scenario describes.

### 2. Scope respected

**Passing:** every file in the diff appears in the group's `Files:` list in `PLAN.md`, or is a test for one of those files. Nothing else changed.

**Fail citing:** cite the file:line of each out-of-scope change. State what it does that the group never asked for.

### 3. Tests real and green

**Passing:** the diff adds or updates tests for its logic-bearing changes. Assertions check observable behavior (return values, state, thrown errors, persisted data). Assertions do not check implementation details (verifying a private call happened, a mock was invoked).

This review actually ran the project's test command. Do not assume green from a report. The command is green. No group is treated as done while red or with tests unrun.

**Fail citing:** cite the file:line of an implementation-coupled assertion. Or cite the actual failing or erroring test output. Or note that no test run could be reproduced for the claimed changes.

### 4. Repo conventions followed

**Passing:** The naming, layering, namespaces, error handling, and file placement match the consuming repo. Match what its `CLAUDE.md` / `AGENTS.md` (or the scout map sourced from it) actually documents. Judge against that repo's stated conventions. Never judge against a style imported from habit or another codebase.

**Fail citing:** cite the file:line of the deviation. Quote the repo's own stated convention it breaks.

### 5. No stray artifacts

**Passing:** the diff contains none of these:

- leftover tool-call tags or agent-output fragments
- AI-assistant/session-mode marker comments (for example `ponytail:`, `caveman:`, or similar mode markers)
- remaining debug output (stray prints/dumps/console logging added for the change itself)
- unexplained stray `TODO`

**Fail citing:** cite the file:line of the artifact. State what kind it is.

### 6. No silent failure paths introduced

**Passing:** every error path the diff adds or touches surfaces its failure. The diff has none of these:

- empty or swallowing `catch` / `except` blocks
- ignored error/return codes
- a fallback that quietly substitutes a default instead of surfacing the problem

**Fail citing:** cite the file:line of the catch/condition. State what failure it hides.

## Decision Rules

- If the target is a ticket, `TASK.md`, or plan before code exists → refuse. This skill reviews written code only.
- If the invocation is per-group → review that group's diff only. That is the commit gate.
- If the invocation is a whole-task sweep → review the combined diff. Do not treat it as the commit gate.
- If an earlier check fails → still run the remaining checks.
- If Shell is unavailable → check 3 is a blocker. Do not mark the gate green.
- If a `DONE` report claims green → still run the test command. Do not trust the report.
- If a finding has no file:line → it is not a finding. Drop it or find the line.
- If a check has no finding → write `N. <name> — clean.`
- Any `blocker` fails the commit gate. `major` / `minor` alone do not fail the gate.
- If the caller asks you to fix the diff → do not edit. Report findings only.

## Output format

List all six checks in order. For a clean check, write one line: `N. <name> — clean.` For a check with findings, list each finding ranked most-severe first:

```
[blocker] <one-line problem> — <file>:<line> — <why it bites>
[major]   <one-line problem> — <file>:<line> — <why it bites>
[minor]   <one-line problem> — <file>:<line> — <why it bites>
```

- **blocker** — fails an AC, breaks scope, ships on red, or hides a failure. It must be fixed before commit.
- **major** — a real quality problem (weak test, convention drift). It should be fixed.
- **minor** — small or arguable. Note it. Do not fail the gate for it.

Example shape:

```
1. Acceptance criteria met — clean.
2. Scope respected — [blocker] <src>/<module>/<file>:42 — not in G2's Files:, pulls in an unrelated refactor
3. Tests real and green — [blocker] no test run reproduced; DONE report claims green but the repo's test command was never invoked this session
4. Repo conventions followed — clean.
5. No stray artifacts — [minor] <src>/<module>/<file>:58 — leftover `// TODO: handle timeout` with no ticket reference
6. No silent failure paths introduced — clean.
```

End with a one-line roll-up. Give the pass/fail count. State whether the group clears the commit gate. Any `blocker` finding fails the gate. `major` / `minor` alone do not fail the gate.

## Constraints

- MUST run all six checks in order.
- MUST cite file and line on every finding.
- MUST run the project's test command for check 3.
- MUST rank findings most-severe first.
- MUST NOT edit, fix, or rewrite any reviewed file.
- MUST NOT skip a check because an earlier one already failed.
- MUST NOT accept a test-green claim without actually running the command.
- MUST NOT assume a coding style, naming rule, or architecture from outside the consuming repo.
- MUST NOT spawn a Task subagent.
- MUST NOT select or hardcode a model.
- MUST NOT use or request `subagent_type: reviewer`.
- NEVER commit, push, or touch git. The orchestrator owns that.

## Boundary against ticket review

**review-implementation** reviews code that has already been written. It reviews a group's diff against its own `TASK.md` / `PLAN.md`, after implementation, before commit.

A ticket-readiness review asks whether the ticket is executable cold, whether its acceptance criteria are testable, and whether scope is bounded. That review runs before any implementation starts. Do not use this skill for that gate.

## Quality Checks

Before you return:

- [ ] The diff, the group block (or full plan), and the acceptance slice were read.
- [ ] All six checks were run in order.
- [ ] Check 3 ran the real test command, or reported Shell unavailable as a blocker.
- [ ] Each finding cites file and line.
- [ ] No reviewed file was edited.
- [ ] The response is findings only, plus the one-line roll-up.
- [ ] Check 4 used this repo's `CLAUDE.md` / `AGENTS.md` / scout map only.
- [ ] Git was not touched.

## Examples

### Out of scope file

Input: the diff edits a file that is not in G2's `Files:` and is not a test for those files.

Expected: `[blocker]` on check 2. Cite file:line. Do not edit the file.

### Trusted DONE report

Input: the implementer returned DONE with a pass summary. Shell is available.

Expected: run the repo's test command anyway. If you cannot reproduce the run, `[blocker]` on check 3.

### Shell unavailable

Input: this session has no Shell. `subagent_type: reviewer` may have been used.

Expected: check 3 is a blocker. Tell the caller to re-dispatch without `subagent_type: reviewer`. Do not mark the gate green.

### Ticket attached

Input: the caller points at `TASK.md` and asks if it is ready to implement.

Expected: refuse. This skill reviews written code only.

### Fix requested

Input: "review this and then fix the blockers".

Expected: report findings. Do not edit. The caller re-dispatches the fix.

## Failure Modes

- Diff or group id missing → stop. Name the required input. Do not guess a group.
- Shell unavailable → check 3 blocker. Ask for re-dispatch without `subagent_type: reviewer`.
- Caller asks to fix → findings only. Do not edit.
- Target is a ticket or plan → refuse. This skill is not a ticket review.
- Finding without a line → drop it or find the line.

## References

- `implement-group` — writes the code this skill reviews. Do not run it here.
- `triage-review-feedback` — judges later PR comments. Do not run it here.
