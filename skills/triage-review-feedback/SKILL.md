---
name: triage-review-feedback
description: >-
  Judge each item in a review round's REVIEW_FEEDBACK.md as accept, decline,
  or partial with a short rationale. Ground the verdict in TASK.md/PLAN.md
  and a scout map. Write that round's REVIEW_TRIAGE.md so fixes can be planned.
  Use when PR/code-review comments need a correctness pass before planning
  fixes. Escalate via NEEDS: scout / NEEDS: architect when a fact or design
  fork is missing. Do NOT use to write POST_REVIEW_PLAN.md, implement fixes,
  merge prior rounds, or fetch GitHub comments (the orchestrator owns fetch
  and dedup).
argument-hint: "[path to REVIEW_FEEDBACK.md]"
aibits:
  deps:
    - ~/skills/asd-ste100
---

# Triage Review Feedback

## Purpose

Decide, per comment, whether the feedback is correct and actionable for this task. Write one **round** `REVIEW_TRIAGE.md`.

Do not plan groups. Do not change application code.

## Activation

### Use when

Use this skill when:

- a round's `REVIEW_FEEDBACK.md` needs a correctness pass before planning fixes
- a caller asks to triage review comments, write `REVIEW_TRIAGE.md`, or judge accept / decline / partial

### Do not use when

Do not use this skill when:

- the task is to write `POST_REVIEW_PLAN.md` (that is `plan-post-review`)
- the task is to implement fixes (that is `implement-group`)
- the task is to merge or re-triage prior rounds
- the task is to fetch platform comments (the orchestrator owns fetch and dedup)
- the task is to review a group's diff before commit (that is `review-implementation`)

## Context

Usual files live under `.ai/task/<task-id>/review/R<n>/`. Write `REVIEW_TRIAGE.md` in that same round directory.

Assume no stack. Read the consuming repo's `CLAUDE.md` and `AGENTS.md`. Read the scout map.

This skill runs inside the triaging agent. Do not spawn a Task. Do not select a model.

The orchestrator already deduped prior-round comment ids from FEEDBACK. Triage only what is in this file. Do not take items from other rounds' triage.

## Input

- Path to this round's `REVIEW_FEEDBACK.md` (under `review/R<n>/`).
- Round id `R<n>`.
- Target path to write `REVIEW_TRIAGE.md` (same round dir).
- Path to `assets/triage-template.md`. Follow its structure exactly.
- `TASK.md` and/or `PLAN.md` when present (intent, scope, what was already built).
- A scout map — real paths, patterns, conventions (may be thin if comments are local).

## Workflow

1. Resolve the round dir, `REVIEW_FEEDBACK.md`, and the target `REVIEW_TRIAGE.md` path.
2. Read `assets/triage-template.md`. Follow its structure exactly.
3. Read this round's `REVIEW_FEEDBACK.md` in full.
4. Read `TASK.md` intent, AC, and out-of-scope. Read `PLAN.md` groups if present.
5. Read the scout map for the cited paths. Read `CLAUDE.md` / `AGENTS.md` for convention checks.
6. For each numbered item, choose exactly one verdict: accept, partial, or decline.
7. Write 1–3 lines of rationale. Copy Platform, Kind, Comment id, and Thread id from FEEDBACK.
8. Fill summary counts. Keep declined items in the file.
9. Write one `REVIEW_TRIAGE.md` at the target path. Write nothing else.

## Verdicts

For each numbered feedback item, choose exactly one:

| Verdict     | When                                                                                                                 |
| ----------- | -------------------------------------------------------------------------------------------------------------------- |
| **accept**  | Correct, in scope for this task, and actionable in the codebase as it stands.                                        |
| **partial** | Partly right. Name what to take and what to drop. Only the taken part is actionable.                                 |
| **decline** | Incorrect, out of scope for this task, duplicates existing intent, or pure style with no repo convention backing it. |

## How to judge

1. **Read the feedback and the task context.** Read `TASK.md` intent/AC/out-of-scope. Read `PLAN.md` groups if present. Read the scout map for the cited paths.
2. **Check the claim against the code** (using scout map / Read). Decline with that reason when a comment does any of these:
   - cites the wrong file
   - misreads behavior
   - asks for something the AC already forbids
3. **Scope.** If the ask belongs to another ticket or a future epic, decline as out of scope. Do not stretch this task.
4. **Conventions.** Style-only feedback must cite a rule from the repo's own `CLAUDE.md` / `AGENTS.md` or scout map to be accept. Otherwise decline (taste is not a convention).
5. **One rationale each.** Write 1–3 lines: why accept / what partial keeps / why decline. No essays.
6. **Fill the template.** Write summary counts. Then write every item with verdict + rationale.
7. **Copy reply ids.** Copy **Platform**, **Kind**, **Comment id**, and **Thread id** from FEEDBACK. The caller needs those ids to reply.
8. Declined items stay in the file. They must not be silently dropped.

## Decision Rules

- If the feedback path cannot be resolved → stop. Name the required input. Do not guess.
- If a code fact is missing → return `NEEDS: scout`. Do not guess a verdict.
- If the review raises a real design fork → return `NEEDS: architect`. Do not pick a design.
- If the comment cites the wrong file or misreads behavior → decline.
- If the ask is outside this task's AC or out-of-scope → decline.
- If style-only feedback has no repo convention behind it → decline.
- If the comment is partly right → partial. Name what to take and what to drop.
- If Comment id or Thread id is in FEEDBACK → copy it. Do not drop it.
- If an item is from another round → ignore it. Triage only this file.
- If a decline would become accept only to be agreeable → keep the decline.

## Escalation contract

If a code fact is missing, or if the review raises a real design fork, stop. Return an escalation. Do not guess a verdict that depends on an unknown:

```
NEEDS: scout
<recon question>
```

— or —

```
NEEDS: architect
<design question>
```

## Constraints

- MUST write only the target `REVIEW_TRIAGE.md`.
- MUST fill every template section.
- MUST give each item exactly one verdict: accept, partial, or decline.
- MUST copy Platform, Kind, Comment id, and Thread id from FEEDBACK when present.
- MUST keep declined items in the file.
- MUST return a `NEEDS: scout` or `NEEDS: architect` block in the shape above when blocked.
- MUST NOT invent feedback items not in `REVIEW_FEEDBACK.md`.
- MUST NOT take or re-triage items from another round's files.
- MUST NOT plan implementation groups.
- MUST NOT change application code.
- MUST NOT spawn a Task subagent.
- MUST NOT select or hardcode a model.
- MUST NOT change a decline into accept to be agreeable.
- NEVER commit, push, or touch git. The orchestrator owns that.
- NEVER fetch platform comments. The orchestrator owns fetch and dedup.

## Output format

Write one `REVIEW_TRIAGE.md` at the target path. Match `assets/triage-template.md`.

If blocked, return only the escalation block. Do not write a guessed triage.

## Quality Checks

Before you finish:

- [ ] This round's `REVIEW_FEEDBACK.md`, `TASK.md` / `PLAN.md`, and the scout map were read.
- [ ] Every item in FEEDBACK has a verdict and a 1–3 line rationale.
- [ ] Summary counts match the items.
- [ ] Platform, Kind, Comment id, and Thread id were copied when FEEDBACK recorded them.
- [ ] Declined items remain in the file.
- [ ] No item was taken from another round.
- [ ] No file other than the target `REVIEW_TRIAGE.md` was written.
- [ ] Git was not touched.

## Examples

### Style with no convention

Input: a comment asks to rename a local variable. `CLAUDE.md` and the scout map state no such rule.

Expected: decline. Taste is not a convention. Keep the item in the file.

### Wrong file

Input: a comment cites a path the scout map shows does not contain the behavior.

Expected: decline. State the misread. Do not accept to be agreeable.

### Partial

Input: a comment asks for a valid guard plus an unrelated refactor.

Expected: partial. Keep the guard. Drop the refactor. Write the actionable ask as the guard only.

### Missing fact

Input: the comment claims a race. The scout map does not cover that path.

Expected: `NEEDS: scout`. Do not guess accept or decline.

### Other round

Input: the caller also attaches `review/R1/REVIEW_TRIAGE.md` while this round is R2.

Expected: ignore R1 items. Triage only this round's FEEDBACK.

## Failure Modes

- FEEDBACK path missing → stop. Name the required input. Do not guess.
- Code fact missing → `NEEDS: scout`. Do not guess a verdict.
- Design fork → `NEEDS: architect`. Do not pick a design.
- Caller asks to plan groups → refuse. Point to `plan-post-review`.
- Caller asks to fetch comments → refuse. The orchestrator owns fetch.

## References

- `assets/triage-template.md` — `REVIEW_TRIAGE.md` structure. Read once per run. Follow it exactly.
- `plan-post-review` — writes `POST_REVIEW_PLAN.md` after this triage is approved. Do not run it here.
- `review-implementation` — the pre-commit code review. Do not run it here.
