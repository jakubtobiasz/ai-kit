# AGENTS.md — Docs

Living notes for future agents. Not binding architecture.

Start at [`_index.md`](./_index.md). Copy structure from [`TEMPLATE.md`](./TEMPLATE.md).

## What belongs here

How a subsystem works after we built it. Non-binding "why we did it this way". One topic per file until the topic grows.

Default at task close-out is write nothing. Promote only when a future agent would need the fact without opening `.ai/task/<id>/`.

## Growth

- Default: `.ai/docs/<topic>.md`
- When a second document appears: `.ai/docs/<topic>/_index.md` plus sibling files. The former file becomes the directory `_index` or a named sibling. Update the root `_index.md` row to point at the directory.

## What does not belong here

| Put it elsewhere | Why |
| --- | --- |
| Binding constraints | [`.ai/adr/`](../adr/_index.md) |
| Ticket execution state | [`.ai/task/`](../task/_index.md) |
| Staging PRDs and tickets | Linear, after push. `.ai/idea/` is deleted |
| One file per ticket key | A ticket is not a topic |
| Board dissent | [`.ai/board-voting/`](../board-voting/INDEX.md) |

## Conventions

| Field | Rule |
| --- | --- |
| Filename | Topic kebab. Never a tracker key |
| Status | `note` \| `promoted-to-ADR` \| `superseded` |
| Index | Add a row to `_index.md` in the same change |
| Language | English STE |
