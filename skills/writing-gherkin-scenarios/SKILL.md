---
name: writing-gherkin-scenarios
description: Write or rewrite the Gherkin block under a ticket's `## Acceptance criteria`. Use declarative Given/When/Then in domain language, in the Sylius style. Use when writing or improving ticket AC, or when a caller asks for Gherkin on a ticket or user story. Triggers include "write acceptance criteria", "write the scenarios", "write this as Gherkin", "add Given/When/Then", and "these acceptance criteria are weak". Do not use for step definitions, test runners, unit or integration tests, Cypress, or suite `.feature` files unless the user asked for ticket AC.
argument-hint: "[ticket file or requirement]"
aibits:
  deps:
    - ~/skills/asd-ste100
---

# Writing Gherkin Scenarios

## Purpose

Write the `## Acceptance criteria` Gherkin so "done" is testable. If every scenario passes, the user story holds. This skill shapes how the scenarios read. It does not automate them.

## Activation

### Use when

- You write or rewrite a ticket's `## Acceptance criteria`.
- A caller asks for Gherkin, Given/When/Then, or scenarios on a ticket or user story.
- Existing ticket AC is imperative, vague, or untestable.

### Do not use when

- The task is step definitions or glue code.
- The task is to run tests.
- The task is unit, integration, or E2E code.
- The task is a suite `.feature` file, unless the user asked for ticket AC.

## Context

The user story (`As a <role>, I want <capability>, so that <benefit>`) is the promise. The scenarios prove that promise.

Follow the Sylius feature style. Read `references/examples.md` when you need a fuller model or a named smell.

Keep Given/When/Then roles short:

- `Scenario:` names the behavior in plain words. Not "Test 3".
- `Given` is the world before the action. Past tense. No user action.
- `When` is the one action under test. One `When` per scenario.
- `Then` is the observable outcome. Not an internal detail.
- `And` / `But` continue the previous step type.

## Workflow

1. Read the ticket. Read the user story. Read `Out of scope` if it exists.
2. If the story, the role vocabulary, or the real edge cannot be read from the ticket, ask the user. Do not invent scope.
3. Name one happy-path behavior. Name the edges that would break it.
4. Draft 2 to 5 scenarios in the role's words. Use concrete data.
5. Apply `Background` or `Scenario Outline` only to remove real duplication.
6. Run the quality checks.
7. Put the block under `## Acceptance criteria`.

## Decision Rules

- If a scenario has two `When` steps → move the first to `Given`, or split into two scenarios.
- If several scenarios share the same `Given` → move that `Given` to `Background`.
- If one scenario has no shared setup → do not add a `Background`.
- If the same behavior has many data rows → use a `Scenario Outline`.
- If there are only two data rows → prefer separate scenarios.
- If a scenario does not trace to `As a / I want / so that` → it belongs to another ticket. Do not keep it here.
- If the ticket has no `Out of scope` → still write the happy path and one real edge. Ask if that edge is unclear.
- If existing AC is imperative → rewrite it. Do not keep selectors, URLs, or "click".
- If the user asks for glue or step definitions → refuse. Point to this skill's Do not use when.

## Constraints

- MUST write declarative steps. Describe behavior, not UI mechanics.
- MUST use the vocabulary of the ticket and of the `<role>`.
- MUST use concrete values (names, counts, emails). Not "a user" or "some data".
- MUST keep one `When` per scenario.
- MUST trace every scenario to the user story.
- MUST NOT use selectors, URLs, button ids, or "click".
- MUST NOT assert internal implementation in `Then`.
- NEVER invent scope, role words, or edges that the ticket does not support. Ask instead.
- SHOULD write 2 to 5 scenarios. Not an exhaustive matrix.
- SHOULD use `Background` and `Scenario Outline` only when they remove duplication.

## Quality Checks

Before you finish:

- [ ] No step contains `click`, a CSS selector, a `#id`, or a URL path.
- [ ] Each scenario has exactly one `When`.
- [ ] Each `Scenario:` name states the behavior. No name is "Test N".
- [ ] Values are concrete (a name, an email, a count). No "a user" / "some data".
- [ ] Each scenario traces to `As a / I want / so that`.
- [ ] The set includes the happy path and one real breaking edge.
- [ ] Shared `Given` is in `Background`. Same behavior with many rows uses `Scenario Outline`.
- [ ] The block sits under `## Acceptance criteria`.

## Examples

### Typical ticket

Input: a story "As a visitor, I want to register, so that I can place orders later."

Expected: 2 to 5 declarative scenarios. Happy path plus a real edge (email already taken, or verification on vs off). Concrete names and emails. No `/register`, no `#submit-btn`.

### Imperative AC

Input:

```gherkin
When I go to "/register"
And I click the "#submit-btn" button
```

Expected: rewrite to intent (`When I want to register a new account` / `And I register this account`). Drop selectors and "click".

### Missing Out of scope

Input: a ticket with a user story and no `Out of scope`.

Expected: write the happy path and one edge you can justify from the story. If the real edge is unclear, ask. Do not enumerate every input.

### Glue code requested

Input: "write the step definitions for these scenarios".

Expected: refuse. This skill writes ticket AC. It does not write glue code.

## Failure Modes

- No user story on the ticket → ask for `As a / I want / so that`. Do not invent a role.
- Role vocabulary is unclear → ask. Do not guess domain words.
- User wants every input enumerated → write 2 to 5 representative scenarios. State that exhaustive cases belong elsewhere.
- User asks for glue, step defs, or a test runner → refuse.
- Story and `Out of scope` conflict → stop. Report the conflict. Do not pick a side.
- Existing AC is a suite `.feature` file and the user did not ask for ticket AC → do not rewrite it as ticket AC.

## References

- `references/examples.md` — Sylius-style models and before/after rewrites. Read it when you need a fuller model, or when you fix a named smell (imperative, vague, two `When`, testing everything).
- Sylius feature suite — the style source. Do not copy suite file layout into a ticket unless the user asked for a `.feature` file.
