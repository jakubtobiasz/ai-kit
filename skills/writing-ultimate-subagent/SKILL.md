---
name: writing-ultimate-subagent
description: Create focused, reliable, and composable AI subagents. Use when designing a new subagent, improving an existing subagent, or turning a specialized responsibility into a reusable autonomous agent.
---

# Subagent Writer

Create subagents that can independently perform a well-defined responsibility and return a useful result to a parent agent.

A good subagent is a **delegated responsibility**, not a persona and not simply a longer Skill.

A subagent defines:

- what responsibility it owns
- when it should be used
- what input it accepts
- what authority it has
- what tools it may use
- what it must not do
- what output it must return
- when it should stop
- how it reports results to the parent agent

Use this distinction:

> **Rule = policy**  
> **Skill = procedure**  
> **Subagent = delegated responsibility**  
> **Reference = knowledge**  
> **Script = execution**

---

# Purpose

When creating or improving a subagent, optimize for:

1. **Focused responsibility** — the subagent owns one coherent problem.
2. **Clear boundaries** — the parent agent knows what the subagent can and cannot do.
3. **Useful autonomy** — the subagent can complete its responsibility without unnecessary supervision.
4. **Predictable output** — the parent agent can reliably consume the result.
5. **Composability** — the subagent can work as part of a larger agent workflow.
6. **Safe delegation** — the subagent does not make decisions outside its mandate.
7. **Efficient context use** — the subagent receives only the context required for its responsibility.
8. **Explicit failure handling** — incomplete work and uncertainty are reported instead of hidden.

A subagent should have a **job**, not merely a personality.

Avoid defining subagents as:

> "You are an expert senior engineer who is very careful..."

Prefer:

> "Analyze the repository for architectural violations and return a prioritized list of findings."

---

# Subagent Structure

Use this structure as the default:

```markdown
---
name: ...
description: ...
---

# Subagent Name

## Responsibility

...

## Use When

...

## Do Not Use When

...

## Inputs

...

## Authority

...

## Workflow

1. ...
2. ...
3. ...

## Decision Rules

...

## Constraints

...

## Output

...

## Handoff

...

## Failure Handling

...

## Examples

...

## References

...