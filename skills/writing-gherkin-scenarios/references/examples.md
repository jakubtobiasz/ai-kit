# Gherkin examples — annotated

Two authentic features to imitate, then a set of before/after rewrites for the smells you will actually hit. All examples are style models for the `## Acceptance criteria` block of a ticket — the writing, not the automation.

## Contents

- [Model 1 — account registration (declarative flow, layered tags)](#model-1--account-registration)
- [Model 2 — cart quantity (shared Background, data variation)](#model-2--cart-quantity)
- [Before / after rewrites](#before--after-rewrites)
  - [Imperative → declarative](#imperative--declarative)
  - [Vague → concrete](#vague--concrete)
  - [Multiple When → split behaviors](#multiple-when--split-behaviors)
  - [Testing everything → one behavior](#testing-everything--one-behavior)
  - [Glue code requested → refuse](#glue-code-requested--refuse)

---

## Model 1 — account registration

Note what it does _not_ say: no field ids, no submit button, no URL. Every step is a business action.

```gherkin
Feature: Account registration
    In order to make future purchases with ease
    As a Visitor
    I need to be able to create an account in the store

    Background:
        Given the store operates on a single channel in "United States"

    Scenario: Registering a new account when the channel requires verification
        Given on this channel account verification is required
        When I want to register a new account
        And I specify the first name as "Saul"
        And I specify the last name as "Goodman"
        And I specify the email as "goodman@gmail.com"
        And I specify the password as "heisenberg"
        And I confirm this password
        And I register this account
        Then I should be notified that new account has been successfully created
        But I should not be logged in

    Scenario: Registering a new account when the channel does not require verification
        Given on this channel account verification is not required
        When I want to register a new account
        And I specify the email as "goodman@gmail.com"
        And I specify the password as "heisenberg"
        And I confirm this password
        And I register this account
        Then I should be notified that new account has been successfully created
        And I should be logged in
```

Why it works:

- **The `Feature` narrative** is the user story verbatim (`In order to … As a … I need to`). The scenarios prove that promise.
- **`Background`** states the one precondition every scenario shares, once.
- **Two scenarios, one axis of difference** — verification on vs off. Each names that difference in its title. Same action, different world, different outcome.
- **`But I should not be logged in`** — `But` reads naturally for a negative outcome. It is still a `Then`.
- **Declarative to the core** — "I register this account" says nothing about _how_. Ship it as a web form or an API endpoint; the scenario is unchanged.

## Model 2 — cart quantity

Shows domain setup in `Background` and the case for a `Scenario Outline`.

```gherkin
Feature: Viewing a total quantity of the cart
    In order to easily determine the number of products I'm about to buy
    As a Visitor
    I want to track the total quantity of the cart

    Background:
        Given the store has a product "PHP T-Shirt"
        And the store has a product "Sylius T-Shirt"
        And the store has a product "Symfony T-Shirt"

    Scenario: Total quantity across several products with various quantities
        When I add 2 products "PHP T-Shirt" to the cart
        And I add 3 products "Sylius T-Shirt" to the cart
        And I add product "Symfony T-Shirt" to the cart
        Then I should see cart total quantity is 6
```

The original file also has one-product and two-product scenarios. When the _only_ thing changing is the numbers and the expected total, that is the signal to collapse them:

```gherkin
Scenario Outline: Cart total quantity reflects the quantity added
    When I add <count> products "PHP T-Shirt" to the cart
    Then I should see cart total quantity is <count>

    Examples:
        | count |
        | 1     |
        | 2     |
```

Same behavior, two data rows, no copy-paste. Keep the multi-product scenario separate — it proves a _different_ behavior (totals sum across products), so it earns its own scenario.

---

## Before / after rewrites

### Imperative → declarative

The single most common defect. The "before" describes clicking through one screen; the "after" describes the requirement.

```gherkin
# Before
Scenario: Login
    Given I open "/login"
    When I type "goodman@gmail.com" into "#username"
    And I type "heisenberg" into "#password"
    And I press "#login-button"
    Then the URL should be "/dashboard"

# After
Scenario: Signing in with valid credentials
    Given I have a registered account with email "goodman@gmail.com"
    When I sign in with email "goodman@gmail.com" and password "heisenberg"
    Then I should be signed in
    And I should be on my account dashboard
```

The outcome `Then I should be signed in` is what the user cares about; `the URL should be "/dashboard"` is one incidental way to observe it and breaks the day the route changes.

### Vague → concrete

Placeholders hide the real acceptance question: _which_ values, and _what_ exact result?

```gherkin
# Before
Scenario: Adding to cart
    Given there is a product
    When I add it to the cart
    Then the cart should be updated

# After
Scenario: Adding a product to an empty cart
    Given the store has a product "PHP T-Shirt"
    When I add product "PHP T-Shirt" to the cart
    Then I should see cart total quantity is 1
    And the cart subtotal should include "PHP T-Shirt"
```

"the cart should be updated" is unfalsifiable — updated how? Concrete values make the `Then` a real assertion.

### Multiple When → split behaviors

A scenario with several `When`s is usually two scenarios wearing a trench coat. Earlier actions are setup (`Given`); the last is the behavior — unless there are genuinely two behaviors, in which case split.

```gherkin
# Before — registration AND first purchase in one scenario
Scenario: New customer buys something
    When I register a new account
    And I add product "PHP T-Shirt" to the cart
    And I complete the checkout
    Then the order should be placed
    And I should receive a confirmation email

# After — the purchase is the behavior; being registered is context
Background:
    Given I have a registered account with email "goodman@gmail.com"
    And the store has a product "PHP T-Shirt"

Scenario: Placing an order for a single product
    Given I have "PHP T-Shirt" in my cart
    When I complete the checkout
    Then the order should be placed
    And I should receive an order confirmation email
```

If registration itself needs proving, that is a _separate_ scenario (see Model 1) — not glued onto the checkout.

### Testing everything → one behavior

Resist enumerating every field and rule in one giant scenario. Split by behavior; keep each focused.

```gherkin
# Before — one scenario trying to prove the whole form
Scenario: Registration form
    When I register with all valid fields
    Then it works
    When I register with a blank email
    Then it fails
    When I register with a taken email
    Then it fails
    When I register with a short password
    Then it fails

# After — one behavior each, named by what it proves
Scenario: Registering with valid details
    When I register with email "goodman@gmail.com" and password "heisenberg"
    Then I should be notified that new account has been successfully created

Scenario: Registering is rejected when the email is already taken
    Given there is a customer account with email "goodman@gmail.com"
    When I register with email "goodman@gmail.com" and password "heisenberg"
    Then I should be notified that this email is already used

Scenario: Registering is rejected when the password is too short
    When I register with email "goodman@gmail.com" and password "pw"
    Then I should be notified that the password must be at least 8 characters
```

Each scenario now fails for exactly one reason, and its name tells you which. That is what makes a failing suite diagnosable instead of a mystery.

### Glue code requested → refuse

This skill writes ticket AC. It does not write step definitions.

Input: "write the Cypress steps / step defs for these scenarios."

Expected: refuse. Point at the skill's Do not use when. Leave the Gherkin unchanged unless the user also asked to improve the AC.
