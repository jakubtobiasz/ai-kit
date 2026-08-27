---
name: review-issue-readiness
description: >-
  Run a fast, deterministic readiness gate over one TASK_NN.md (or every
  ticket file in an .ai/idea/<slug>/ folder) before you declare it ready
  for implementation. Use when someone asks "is this ticket ready",
  "check readiness", "gate this issue", "can we start this", "is this
  good to hand off", or before implementation starts. Run a fixed
  checklist — executable cold, testable acceptance criteria, explicit
  acyclic dependencies, bounded scope, no invented decisions. Return
  pass/fail per check with findings that cite file and section. Never
  edit the file. For an on-demand adversarial deep review that searches
  for assumptions, hidden risk, and edge cases beyond this checklist, use
  grill-me instead.
argument-hint: "[TASK_NN.md file or .ai/idea/<slug>/ dir]"
aibits:
  deps:
    - ~/skills/writing-gherkin-scenarios
    - ~/skills/asd-ste100
---

# Review Issue Readiness

## Purpose

Run a routine gate on every ticket before you declare it ready. Use the same five checks every time. Weak tickets are caught systematically. They are not caught only when someone thinks to ask for a grill.

This skill reads the ticket and reports pass or fail. It never rewrites or fixes anything.

## Activation

### Use when

Use this skill when:

- a human asks if a ticket is ready, if they can start it, or if it is good to hand off
- a caller asks to check readiness or gate an issue
- implementation is about to start and the ticket has not passed this gate

### Do not use when

Do not use this skill when:

- the task is to rewrite or fix the ticket. Return findings only.
- the task is an adversarial deep review. Use `grill-me`.
- the task is to write the ticket body. Use `write-issue`.
- the task is to slice the PRD. Use `split-prd-into-issues`.
- the target is a PRD, TECH_PRD, or plan rather than a `TASK_NN.md`. Use `grill-me` for those artifacts.
- the task is a code review.

## Context

Input is one `TASK_NN.md`, or every `TASK_NN.md` in an `.ai/idea/<slug>/` folder.

When the input is a folder, run all five checks independently against each file. Also run the cross-file dependency check across the whole set (see check 3).

This is the fast, deterministic gate. It does not interpret. It does not search for hidden risk. It does not attack from open-ended angles. It only confirms the fixed minimum bar is met.

`grill-me` is the on-demand adversarial deep review. Invoke it when someone explicitly wants a hard stress-test. Do not run it routinely on every ticket.

The `writing-gherkin-scenarios` bar is applied here as a check, not as a rewrite.

## Workflow

1. Resolve the target from the argument, the message, or attached files.
2. Read the ticket file itself. For checks 3 and 5, also read its parent (`EPIC.md` / `STORY.md`). Also read any sibling `TASK_NN.md` it depends on or that depends on it.
3. Run all five checks below, in order, against every file. Do not skip a check because an earlier one failed.
4. Report pass or fail per check, per file. For every fail, cite the file and section and quote the offending text.
5. Do not edit the file. Do not suggest a rewrite. Findings only. The caller decides the fix.
6. When the target is a folder, add a one-line roll-up after the per-file blocks.

## The checklist

### 1. Executable cold by a stranger

**Passing:** the `Description` and `Technical notes` give a reader with no memory of the conversation that produced the ticket enough concrete context — real file, module, or service names, actual constraints, not "as discussed" or "the usual way" — to start work without a clarifying question to the author.

**Fail citing:** quote the vague or context-dependent line. Name the section it is in. Say specifically what a stranger would have to guess or ask.

### 2. Acceptance criteria concrete and falsifiable

The `writing-gherkin-scenarios` bar, applied as a check rather than a rewrite.

**Passing:** every `Scenario` has exactly one `When`, uses concrete real values (not "a user" / "some data"), states an observable `Then`, and could be marked pass or fail by someone other than the author without asking what a term meant.

**Fail citing:** quote the scenario and the vague or untestable term (for example "works", "handles it correctly", a placeholder value). Name which part of the bar it misses (declarative, one behavior, concrete values, or observable outcome).

### 3. Dependencies explicit and acyclic

**Passing:** the `Depends on` field lists every real dependency (or states `none`), every listed dependency resolves to a file that actually exists in the idea folder, and following the dependency chain from this ticket never returns to itself.

**Fail citing:** name the missing dependency (a coupling visible in the `Description` or `Technical notes` that is not reflected in `Depends on`), the dependency that points nowhere, or the cycle (list the chain of files that loops).

### 4. Scope bounded with a real Out-of-scope section

**Passing:** the ticket has an `## Out of scope` section that names genuine adjacent-but-excluded work. It is specific enough that someone tempted to expand scope would recognize they have crossed the line.

**Fail citing:** cite the section as missing, empty, or containing only a line that excludes nothing concrete (for example "anything else" or "future work").

### 5. No invented product decisions

**Passing:** every product or behavior decision in the ticket traces to a stated source — this ticket's own `Description` / `Acceptance criteria`, its parent (`EPIC.md` / `STORY.md`), a `PRD.md`, or an explicit prior decision recorded elsewhere in the idea folder. Nothing reads like the ticket-writer filled a gap with their own unstated judgment call.

**Fail citing:** quote the decision, the section it is in, and say what source it fails to trace to.

## Output format

For each file reviewed, list all five checks with PASS or FAIL. For every FAIL, include the finding: file, section, quoted text, and what is wrong. No suggested rewrite. Example shape:

```text
TASK_03.md
1. Executable cold — PASS
2. Testable AC — FAIL — ## Acceptance criteria, Scenario "Guest places an order": "the order succeeds" has no observable Then — a stranger cannot tell what "succeeds" means.
3. Dependencies — PASS
4. Scope bounded — PASS
5. No invented decisions — PASS
```

When run over a folder, report one such block per file, then a one-line roll-up (for example "3/4 files pass all five checks. TASK_02.md fails check 3 — circular dependency with TASK_04.md").

## Decision Rules

- If the target is one `TASK_NN.md` → run the five checks on that file.
- If the target is `.ai/idea/<slug>/` → run the five checks on every `TASK_NN.md`. Then run the cross-file dependency check.
- If the path is absent and cannot be inferred → stop. State that a `TASK_NN.md` or `.ai/idea/<slug>/` directory is required.
- If a listed file cannot be read → fail the checks that need it. Continue with the files you did read.
- If an earlier check fails → still run the remaining checks.
- If the caller asks to rewrite or fix → do not edit. Report findings only.
- If the caller wants an adversarial deep review → point to `grill-me`. Do not expand these five checks.
- If Gherkin is missing entirely → fail check 2. Quote the missing `## Acceptance criteria` section.

## Constraints

- MUST run all five checks on every target file.
- MUST cite file, section, and quoted text on every FAIL.
- MUST NOT edit the file.
- MUST NOT suggest a rewrite.
- MUST NOT skip a later check because an earlier one failed.
- MUST NOT expand the checklist with `grill-me` angles.
- NEVER attack the author.
- SHOULD apply the `writing-gherkin-scenarios` bar as a check when Gherkin is present.

## Quality Checks

Before you return:

- [ ] The target was read, including parent and siblings when checks 3 and 5 need them.
- [ ] All five checks ran, in order, on every file.
- [ ] Each FAIL cites file, section, quoted text, and what is wrong.
- [ ] No file was edited.
- [ ] The response is findings only. No rewrite.
- [ ] A folder run includes a one-line roll-up.
- [ ] `grill-me` was not run as a substitute for this gate.

## Examples

### Untestable AC

Input: a ticket AC line `Then the order succeeds`.

Expected: check 2 FAIL. Quote the line. Name the missing observable outcome. Do not rewrite the scenario.

### Folder with a cycle

Input: `.ai/idea/guest-checkout/` where `TASK_02.md` depends on `TASK_04.md` and `TASK_04.md` depends on `TASK_02.md`.

Expected: both files fail check 3. The roll-up names the cycle. Other checks still run.

### Rewrite requested

Input: "gate this issue and fix the AC".

Expected: run the five checks. Return findings. Do not edit.

### Missing target

Input: "is this ready" with no file and no directory in the conversation.

Expected: stop. State that a `TASK_NN.md` or `.ai/idea/<slug>/` directory is required.

## Failure Modes

- Target path missing and not inferable → stop. Name the required input. Do not guess.
- Listed file unreadable → fail the checks that need it. Continue with files you read.
- Caller asks to rewrite → findings only. Do not edit.
- Caller wants a grill → point to `grill-me`. Do not expand this checklist.
- Artifact is a PRD or plan, not a ticket → refuse this skill. Point to `grill-me`.

## References

- `writing-gherkin-scenarios` — Gherkin AC bar applied as check 2. Read when Gherkin is present.
- `grill-me` — adversarial deep review beyond these five checks. Use only on request.
- `write-issue` — ticket bodies. Not part of this procedure.
