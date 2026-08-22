---
name: writing-gherkin-scenarios
description: Write clear, testable Gherkin acceptance criteria for a ticket — declarative Given/When/Then scenarios in real domain language, in the Sylius style. Use whenever you are writing or improving the `## Acceptance criteria` of a ticket or user story, drafting `Scenario:` blocks, or turning a requirement into behavior examples. Triggers include "write acceptance criteria", "write the scenarios", "write this as Gherkin", "add Given/When/Then", "these acceptance criteria are weak", and the write-issue skill drafting a ticket's AC block. Teaches declarative-over-imperative phrasing, one-behavior-per-scenario, Background and Scenario Outline, and concrete test data. Do NOT use for implementing step definitions / glue code, running tests, or writing non-Gherkin test code (unit / integration).
argument-hint: "[ticket file or requirement]"
license: MIT
metadata:
  author: atlas
  version: "1.0.0"
---

# Writing Gherkin Scenarios

Acceptance criteria are behavior examples a non-programmer can read and agree with. The ticket's user story (`As a <role>, I want <capability>, so that <benefit>`) is the promise. The scenarios are the proof. Write them so that if every scenario passes, the promise is kept. Write them so that a stranger who starts the ticket knows exactly what "done" means.

This skill shapes the `## Acceptance criteria` Gherkin block inside a ticket. It is about _how the scenarios read_, not how they are automated. No step definitions. No test-runner glue. The gold standard for this style is the [Sylius](https://github.com/Sylius/Sylius) feature suite. `references/examples.md` has annotated examples and before/after rewrites.

## The rule that matters most: declarative, not imperative

Describe **what behavior** happens. Never describe **how the UI does it**. If a scenario mentions a button id, a CSS selector, a URL, or "click", it has stopped describing the requirement. It has started describing one fragile implementation of it.

```gherkin
# Imperative — brittle, UI-coupled, says nothing about intent
When I go to "/register"
And I fill in "#customer_email" with "goodman@gmail.com"
And I click the "#submit-btn" button

# Declarative — reads as the requirement, survives any redesign
When I want to register a new account
And I specify the email as "goodman@gmail.com"
And I register this account
```

Both describe the same click-path. Only the second one still makes sense if the form moves, the button is renamed, or the feature ships as an API. That is the whole Sylius lesson: a scenario is a specification, not a script.

## Anatomy of a scenario

- **`Scenario:` name** — states the behavior being proven, in plain words. "Registering with an email that is already taken", not "Test 3" or "Email validation".
- **`Given`** — the world _before_ the action: existing state and context. Past tense, no user action. ("the store has a product \"PHP T-Shirt\"".)
- **`When`** — the _single_ action under test. One `When` per scenario. If you need two `When` steps:
  - the first is really setup (move it to `Given`)
  - or you are testing two behaviors (split the scenario)
- **`Then`** — the observable outcome. What the user or system can _see_ changed. Never an internal implementation detail.
- **`And` / `But`** — continue the previous step type without repeating the keyword.

The shape is: _given this context, when this one thing happens, then this is true._ Keeping `When` to a single action is what keeps a scenario about one behavior.

## Speak the domain's language

Use the vocabulary from the ticket and the business. Use the same words the `<role>` in the user story would use. A scenario about a "cart" and a "customer" must not use "the CartService" and "the user row". Consistency here lets a product owner read the AC and catch a wrong assumption.

Use **concrete, real values**, not placeholders. "Saul Goodman", "PHP T-Shirt", "2 products" — not "a valid user", "some product", "the right quantity". Concrete data makes the expected outcome unambiguous. It also makes the scenario runnable as-is.

## One scenario, one behavior

Do not try to prove everything in one scenario. Do not enumerate every input. Cover:

1. The **happy path** — the capability working as the user story promises.
2. The **edge that would actually break it** — use the ticket's `Out of scope`, risks, and the validation/error cases that matter. (Empty state, already-exists, over-limit, unauthorized — whichever is real for this ticket.)

Representative, not exhaustive. Two to five sharp scenarios beat fifteen that restate each other. Every scenario should trace to the `As a / I want / so that`. If one does not, it belongs to a different ticket.

## Repeated setup → `Background`. Varying data → `Scenario Outline`

When several scenarios start with the same `Given`, move it to a `Background`. State the shared context once:

```gherkin
Background:
  Given the store has a product "PHP T-Shirt"

Scenario: Adding a single product to the cart
  When I add product "PHP T-Shirt" to the cart
  Then I should see cart total quantity is 1
```

When the _same behavior_ is proven against several data rows, replace the copy-paste with a `Scenario Outline` and `Examples`:

```gherkin
Scenario Outline: Cart total quantity reflects the quantity added
  When I add <count> products "PHP T-Shirt" to the cart
  Then I should see cart total quantity is <count>

  Examples:
    | count |
    | 1     |
    | 2     |
    | 6     |
```

Use these only when they remove real duplication. A single scenario does not need a `Background`. Two data rows rarely need an `Outline`.

## Quality bar

Before you declare the AC complete, check each scenario against this:

- [ ] **Declarative** — no selectors, URLs, buttons, or "click". Describes intent, not mechanics.
- [ ] **Domain language** — the words the `<role>` would use. Consistent with the ticket.
- [ ] **One behavior** — one `When`. The `Scenario:` name states that behavior.
- [ ] **Concrete values** — real names and numbers, not "a user" / "some data".
- [ ] **Traceable** — traces to the user story's `As a / I want / so that`.
- [ ] **Covers the break** — happy path _and_ the edge that would realistically fail.
- [ ] **No duplication** — shared `Given` in `Background`, varying data in a `Scenario Outline`.

## More examples

`references/examples.md` — annotated Sylius-style scenarios and a set of before/after rewrites (imperative→declarative, vague→concrete, multi-When→split, testing-everything→one-behavior). Read it when you want a fuller model to imitate or a specific smell to fix.
