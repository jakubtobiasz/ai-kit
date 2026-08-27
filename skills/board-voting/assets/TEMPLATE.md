# BV-NNNN: <short title>

- **Status:** Decided | Superseded by BV-NNNN | Reopened
- **Date:** YYYY-MM-DD
- **Proposal:** one-sentence statement of what was on the table
- **Proposer:** who raised it (human / agent / ticket)
- **Board:** comma-separated seat names as convened (example default: KIMI K3, Opus, Sol)
- **Models used:** exact Task model slugs that cast votes (note substitutions if requested Medium/etc. was unavailable)
- **Related paths:** optional code / docs touched by the proposal

## Proposal

What change was proposed. Keep concrete (files, patterns, alternatives a/b/c).

## Proposer rationale

Why the proposer wanted it — pros and acknowledged cons, in their words or a faithful paraphrase.

| + / − | Point |
| ----- | ----- |
| +     | …     |
| −     | …     |

## Votes

| Seat | Model slug | Verdict                          | One-line rationale |
| ---- | ---------- | -------------------------------- | ------------------ |
| …    | …          | Adopt / Defer / Reject / Abstain | …                  |

### <Seat name>

- **Verdict:** Adopt | Defer | Reject | Abstain
- **Rationale:**
  - …
- **Preferred alternative (if not Adopt):** …
- **Revisit when:** …

<!-- Repeat ### <Seat> for each board member. -->

## Tally

| Verdict | Count |
| ------- | ----- |
| Adopt   | N     |
| Defer   | N     |
| Reject  | N     |
| Abstain | N     |

- **Voters casting:** N
- **Quorum:** met | not met (example default board is 3 seats. Record if a seat was skipped)

## Liberum veto

- **Offered:** yes | no
- **Exercised:** yes | no
- **Board decision before veto:** Adopt | Defer | Reject | n/a (not exercised)
- **Outcome after veto:** Adopt (proposer proposal stands) | n/a (not exercised)
- **Notes:** e.g. human declined. Mechanism not yet introduced. Applied later on YYYY-MM-DD

Votes and tally above stay intact even when veto is exercised. Veto overrides the **Decision** only.

## Decision

- **Decision:** Adopt | Defer | Reject
- **Decision rationale:** chair synthesis. When exercised: “Adopt via liberum veto. Board had \<verdict\>.”
- **Binding effect:** none (advisory) | constrains practice until revisited | recommend ADR
- **Follow-ups:** optional bullets (extract, helper, ADR draft, implementation consensus, do-nothing, …)

## Implementation consensus

Fill **only** when liberum veto was exercised. Otherwise write `n/a — liberum veto not exercised`.

Board reconvenes on **how** to implement the Adopted proposal (whether is closed).

| Seat | Model slug | Position (implementation shape) | One-line rationale |
| ---- | ---------- | ------------------------------- | ------------------ |
| …    | …          | …                               | …                  |

### Consensus

- **Agreed shape:** concrete files, patterns, project architecture constraints
- **Explicit non-goals:** what this adoption still does not include
- **Open points (if any):** unresolved items left for the human

## When to revisit

One line: concrete trigger (size threshold, new sub-resource, contradictory evidence), not a calendar date.

## Links

- Skill: board-voting skill (consuming-repo install path, often `.cursor/skills/board-voting/SKILL.md`)
- Index: [`INDEX.md`](./INDEX.md)
- Related ADR / task / PR: …
