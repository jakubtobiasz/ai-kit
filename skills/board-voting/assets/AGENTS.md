# AGENTS.md — Board voting

Multi-model deliberation records for design forks that are not (yet) ADRs.

Start at [`INDEX.md`](./INDEX.md). Run votes via the board-voting skill. Copy structure from [`TEMPLATE.md`](./TEMPLATE.md).

If this directory is missing files, copy them from the board-voting skill `assets/` (`AGENTS.md`, `TEMPLATE.md`, `INDEX.md`).

## What belongs here

Opinions from a convened board on a concrete proposal — each seat’s verdict and rationale, the tally, optional **liberum veto**, a chair **Decision**, and (after veto) **implementation consensus**.

Default seats live in the board-voting skill (example default: KIMI K3, Opus, Sol — each seat is an `architect` agent with that seat’s model). The consuming repo or the human MAY override seats.

Use board voting when:

- Several reasonable approaches compete and you want explicit multi-model dissent on the record
- You want a lightweight, searchable “we already debated this” trail without promoting an ADR yet
- The human asks to “put it to the board” / “board vote” / similar

### Liberum veto

After every live vote, the orchestrator asks the human whether to exercise **liberum veto**.

| If… | Then… |
| --- | --- |
| Declined | Final Decision is the board Decision. Then persist votes and Decision. |
| Exercised | Votes stay on record. Decision becomes **Adopt** (proposer proposal stands). The board reconvenes for **implementation consensus** (how, not whether). |

Liberum veto never deletes dissent. INDEX may show `Adopt (liberum veto)`.

## What does not belong here

| Put it elsewhere                         | Why                            |
| ---------------------------------------- | ------------------------------ |
| Binding architecture constraints         | [`.ai/adr/`](../adr/AGENTS.md) |
| Ticket refine / plan / review scratch    | [`.ai/task/`](../task/)        |
| Product / tech PRDs and issue breakdowns | [`.ai/idea/`](../idea/)        |

A board vote is **advisory** unless the Decision says otherwise. Promote lasting constraints to an ADR. Then mirror them in project architecture rules when they bind day-to-day code.

## Conventions

| Field    | Rule                                                                                 |
| -------- | ------------------------------------------------------------------------------------ |
| Filename | `NNNN-kebab-title.md` (zero-padded, monotonically increasing)                        |
| Status | `Decided` once persisted. `Superseded by BV-NNNN` or `Reopened` if revisited |
| Scope | One proposal per file |
| Template | Fill every section in [`TEMPLATE.md`](./TEMPLATE.md). Do not invent a parallel shape |
| Index | Add a row to [`INDEX.md`](./INDEX.md) in the same change |
| Models | Record exact Task slugs used. Note if a requested effort or slug was unavailable |
| Veto | Record Offered/Exercised. Keep Votes + Tally even when Decision is overridden |

Do not rewrite vote rationales. Supersede or reopen with a new `BV-NNNN`. Or apply liberum veto / implementation consensus as additive sections per the skill.
