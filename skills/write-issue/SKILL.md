---
name: write-issue
description: >-
  Write one ticket's full body into its TASK_NN.md file — title,
  problem/context, scope, Gherkin acceptance criteria, out of scope,
  dependencies, and test expectations — for a single already-scoped slice
  of work. Use once a PRD or refined idea is sliced and one slice is ready
  to write as a complete ticket. Triggers include "write the ticket",
  "draft this issue", "write up this task", "turn this slice into a task",
  and "flesh out TASK_NN". Fill the staging TASK_NN.md file in
  .ai/idea/<slug>/. Push sends it to Linear and then deletes the slug.
  Use the task-template.md shape from split-prd-into-issues. Use the
  writing-gherkin-scenarios skill for acceptance criteria. Do NOT use it
  to decide how work is sliced, ordered, or shaped into single ticket /
  story / epic (that is split-prd-into-issues). Do NOT use it to judge
  whether a ticket is ready to hand off (that is review-issue-readiness).
argument-hint: "[path to scope slice or TASK_NN.md]"
aibits:
  deps:
    - ~/skills/writing-gherkin-scenarios
    - ~/skills/split-prd-into-issues
    - ~/skills/asd-ste100
    - ~/skills/ai-dir
---

# Write Issue

## Purpose

Write one ticket's content into its `TASK_NN.md` file. Fill title, problem and context, scope, acceptance criteria, out of scope, dependencies, and test expectations.

This skill owns ticket content only. It does not decide how work is sliced, numbered, or shaped. That is `split-prd-into-issues`. It does not judge whether the result is ready to hand off. That is `review-issue-readiness`.

## Activation

### Use when

Use this skill when:

- a PRD or refined idea is already sliced
- one slice is ready to write as a complete ticket
- a human asks to write, draft, or flesh out a `TASK_NN.md`

### Do not use when

Do not use this skill when:

- the work is not yet sliced. Use `split-prd-into-issues`.
- the task is to judge readiness. Use `review-issue-readiness`.
- the task is to grill the ticket. Use `grill-me`.
- the task is to create tracker issues. Use `push-issues`.
- the task is to write a PRD. Use `write-prd` or `write-tech-prd`.

## Context

Required input:

- An approved `PRD.md` or `TECH_PRD.md`, or a refined idea that is already settled. Not a raw brainstorm.
- The selected scope slice: the specific piece of work this ticket must cover, and where it sits relative to any sibling tickets or dependencies.
- The existing `TASK_NN.md` file. This skill writes in place. It does not create a new file.
- Repository context. Use the code, conventions, and constraints needed to ground the ticket, so an executor can start work without guessing.

Use `../split-prd-into-issues/assets/task-template.md` for the file's shape. Do not re-describe its layout.

Draft acceptance criteria with the `writing-gherkin-scenarios` skill. Do not draft them freehand. Use declarative Given/When/Then. Put one behavior in each scenario. Use concrete values. Cover the happy path plus the edge that would actually break it.

A staging ticket is not a durable bucket. Linear is the source of truth after `push-issues`. Write the draft in English STE. Do not translate it into the human's chat language. See `skills/asd-ste100` and `rules/asd-ste100.md`. Match the human's language in live chat only.

## Workflow

1. Resolve the target `TASK_NN.md`. If the file does not exist, stop. Point to `split-prd-into-issues`.
2. Read the approved PRD or TECH_PRD. Read the parent file if one exists. Read sibling tickets that this slice depends on.
3. Read the selected scope slice. Confirm it is one independently verifiable outcome.
4. Read `../split-prd-into-issues/assets/task-template.md` for the body shape.
5. Read the `writing-gherkin-scenarios` skill. Follow it for the `## Acceptance criteria` block.
6. Write the ticket in place from the input:
   - a real title
   - the problem and context an executor needs, in `As a <role>, I want <capability>, so that <benefit>` form, plus whatever else they would otherwise have to ask you for
   - the scope this ticket covers
   - acceptance criteria from `writing-gherkin-scenarios`
   - what is deliberately excluded
   - its dependencies on other tickets
   - what proves the ticket done
7. Run the quality checks. Do not call any tracker API.

## Decision Rules

- If `TASK_NN.md` does not exist → stop. Point to `split-prd-into-issues`. Do not create the file.
- If the slice cannot be marked as a single done or not-done → say so. Do not force it into one ticket.
- If the PRD or the scope slice is unclear or silent → report that gap in the ticket, or ask. Do not silently pick an interpretation.
- If a product decision is missing → flag it as an ambiguity. Do not invent the decision.
- If existing AC is imperative → rewrite it with `writing-gherkin-scenarios`. Do not keep selectors, URLs, or "click".
- If the human asks to grill or push in the same turn → finish the ticket body first. Do not run those skills here.

## Constraints

- MUST write English STE. See `skills/asd-ste100` and `rules/asd-ste100.md`.
- MUST NOT translate the ticket into the human's chat language.
- MUST write in the existing `TASK_NN.md`. MUST NOT invent a new file.
- MUST draft acceptance criteria with `writing-gherkin-scenarios`. MUST NOT draft them ad hoc.
- MUST keep one independently verifiable outcome per issue.
- MUST name everything the ticket touches in description or scope.
- MUST name everything it deliberately does not touch in out of scope.
- MUST flag PRD ambiguities instead of resolving them.
- MUST NOT invent product decisions.
- MUST NOT call a tracker API.
- NEVER hide scope.

## Quality Checks

Before you declare a ticket done:

- [ ] The file already existed. This skill did not create it.
- [ ] The ticket is one independently verifiable outcome.
- [ ] Title, description, scope, out of scope, dependencies, and test expectations are filled.
- [ ] Acceptance criteria were drafted with `writing-gherkin-scenarios`.
- [ ] No hidden scope. Touched work is named. Excluded work is named.
- [ ] PRD gaps are flagged. No invented product decisions.
- [ ] The ticket is English STE. It is not translated into the human's chat language.
- [ ] No tracker API was called.

## Examples

### Typical slice

Input: `.ai/idea/guest-checkout/TASK_00.md` for "guest can place an order without an account".

Expected: fill that file in place. User story plus Gherkin happy path and one real edge (for example an invalid email). Concrete names and totals. No `/checkout`. No `#submit`.

### File missing

Input: "write TASK_01" but no `TASK_01.md` exists.

Expected: stop. Point to `split-prd-into-issues`. Do not create the file.

### Two outcomes in one slice

Input: a slice that both places the order and sends the reset-password email.

Expected: say the slice is more than one ticket. Do not force it into one `TASK_NN.md`.

### Silent PRD gap

Input: the PRD never says what happens when payment fails.

Expected: flag the gap on the ticket. Do not invent a payment-failure policy.

## Failure Modes

- `TASK_NN.md` is missing → stop. Point to `split-prd-into-issues`.
- The idea is a raw brainstorm → stop. A settled PRD or refined idea is required.
- The slice covers more than one done or not-done outcome → refuse to force it. Say so.
- Role vocabulary is unclear → ask. Do not guess domain words.
- The human asks for glue or step definitions → refuse. Point to `writing-gherkin-scenarios` Do not use when.

## References

- `../split-prd-into-issues/assets/task-template.md` — ticket file shape. Read before you write the body.
- `writing-gherkin-scenarios` — Gherkin AC. Read on every run. Do not duplicate that skill here.
- `split-prd-into-issues` — slicing and skeleton. Use when the file does not exist.
- `review-issue-readiness` — readiness gate after the body is written. Not part of this procedure.
- `grill-me` — adversarial review. Not part of this procedure.
- `skills/asd-ste100` and `rules/asd-ste100.md` — English STE on the staging draft
- `skills/ai-dir/SKILL.md` — bootstrap and gitignore `.ai/idea/`
