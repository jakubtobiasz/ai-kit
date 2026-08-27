# AGENTS.md — Architecture Decision Records

Binding architecture constraints.

Start at [`_index.md`](./_index.md). Copy structure from [`TEMPLATE.md`](./TEMPLATE.md).

If this directory is missing files, copy them from the `ai-dir` skill `assets/adr/`.

## What belongs here

A constraint implementers MUST follow. One decision per file. After persist, mirror it in a Cursor rule when it binds day-to-day code.

## What does not belong here

| Put it elsewhere | Why |
| --- | --- |
| Living notes, non-binding | [`.ai/docs/`](../docs/_index.md) |
| Advisory board votes | [`.ai/board-voting/`](../board-voting/INDEX.md) |
| Ticket refine / plan / review | [`.ai/task/`](../task/_index.md) |
| Staging PRDs and tickets | Linear |

A board vote MAY recommend an ADR. A doc MAY be promoted to an ADR. Do not file a note or a BV here.

## Conventions

| Field | Rule |
| --- | --- |
| Filename | `NNNN-kebab-title.md` (zero-padded, monotonically increasing) |
| Status | `Accepted` once persisted. `Superseded by ADR-NNNN` if replaced |
| Index | Add a row to `_index.md` in the same change |
| Language | English STE |
