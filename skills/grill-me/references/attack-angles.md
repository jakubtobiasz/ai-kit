# Attack angles — probe questions

Read this after you read the artifact. The finding bar and the cost rank live in `SKILL.md`. Use this file for probe questions only.

Do not invent findings to fill an angle. If the angle is solid, write one line and continue.

---

## Assumptions

Look for claims written as facts with no proof.

Probe:

- Which statement would change the design if it were false?
- Is a volume, rate, or data shape assumed and never sourced?
- Does the artifact assume one vendor, one locale, one role, or one environment?
- Does "users already have X" appear with no check that X exists?
- Does a TECH_PRD assume an API, table, or flag that no ticket creates?

Finding shape: quote the claim. State the break if it is false. Ask what evidence is required, or name the fallback.

## Completeness / gaps

Check these, in this order:

1. **States** — empty, error, loading, partial success, timeout, retry.
2. **Roles** — who can do this, who cannot, who is unauthenticated.
3. **Edge cases** — zero rows, max rows, duplicate, stale data, concurrent edits.
4. **Non-functional needs** — performance, security, accessibility. Flag only when the feature type requires them and the artifact is silent.
5. **Missing work** — a ticket or step that must exist for the stated outcome, and does not.

Probe:

- What happens with no data?
- What happens when the dependency fails?
- Who is out of the happy-path role?
- Which error does the user see, and who owns that ticket?
- Does `PRD.md` promise a capability that no ticket covers?
- Does a ticket assume a prior step that has no ticket?

Do not demand every possible edge. Demand the edges that would ship a broken state for this feature.

## Testability

Each acceptance criterion must be concrete and falsifiable. A tester who never met the author must be able to pass or fail it.

Flag phrases such as "works", "is fast", "handles errors gracefully", "is intuitive", "is consistent", "as expected".

For Gherkin AC, apply the `writing-gherkin-scenarios` bar:

- **Declarative** — no selectors, URLs, buttons, or "click".
- **One behavior** — one `When`. The `Scenario:` name states that behavior.
- **Concrete values** — real names and numbers, not "a user" or "some data".

Probe:

- What measurement would fail this criterion?
- What row count, time bound, status code, or visible text is missing?
- Does one scenario test two behaviors?
- Do two tickets share the same AC with different meanings?

Finding shape: quote the criterion. Name the missing check. Suggest the concrete value or the split.

## Scope

Look for work that is in, out, or hidden.

Probe:

- Which line is written as a small change and implies a new system?
- Is "support X" unbounded (every format, every locale, every role)?
- Does Out of scope conflict with an AC line?
- Does a parent PRD include a capability that a child ticket silently drops?
- Does one AC require design, migration, backfill, and UI with no split?

Finding shape: quote the line. Estimate the concealed work. Ask what is in and what is out.

## Dependencies & order

Look at coupling between tickets and between a ticket and its parent.

Probe:

- Which ticket cannot start until another ticket lands, with no `depends on`?
- Which two tickets edit the same surface and have no order?
- Is there a cycle (A needs B, B needs A)?
- Does a ticket consume an API, field, or flag that another ticket never creates?
- Does TECH_PRD sequence contradict ticket order?

Finding shape: name both sides of the coupling. State the stall. Suggest the missing `depends on` or the order.

## Feasibility & risk

Look for the part most likely to fail.

Probe:

- What is unknown (vendor limit, data quality, permission model, migration size)?
- What is sized as a small ticket and needs a spike?
- What has no rollback?
- What requires a person or system the artifact does not name?

Finding shape: quote the optimistic line. Name the failure mode. Ask for a spike, a bound, or an owner.

Do not invent technical risk that the text does not imply. Tie the risk to a line.

## Clarity

Ask: can a new reader execute this without the author?

Probe:

- Which noun has two meanings in this artifact?
- Which step says "handle" or "support" with no action?
- Which role, path, or environment is unnamed?
- Where would an implementer have to guess a default?

Finding shape: quote the stall point. State the guess a stranger would make. Name the missing default or definition.

Skip nits that would not change the work. Clarity findings must cause a stall or a guess.

## Contradictions

Look for two statements that cannot both be true.

Probe:

- Does AC contradict Out of scope?
- Does a ticket contradict `PRD.md` or `TECH_PRD.md`?
- Do two tickets specify different outcomes for the same action?
- Does a count, role, or status change between sections with no note?

Finding shape: quote both lines. State the two valid readings. Ask which one wins.
