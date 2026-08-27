# AGENTS.md — `.ai/` layout

Start here before you write under `.ai/`. Copy this file from the `ai-dir` skill assets if it is missing.

Linear owns tickets. This tree owns execution state and lasting notes.

## Durable buckets

| Path | Job | Catalog |
| --- | --- | --- |
| [`task/`](task/_index.md) | Execution and resume. `task-id` is the tracker key after push. | `_index.md` |
| [`docs/`](docs/_index.md) | Living notes. Not binding. | `_index.md` |
| [`adr/`](adr/_index.md) | Binding architecture. | `_index.md` |
| [`board-voting/`](board-voting/INDEX.md) | Advisory multi-model votes. | `INDEX.md` |

## Staging (not a bucket)

`.ai/idea/<slug>/` is a draft window for PRDs and ticket files. Gitignore it. After a successful push, delete the slug. Do not resume `develop` or `deliver-story` from it.

If no tracker MCP is connected, stop. Do not treat the drafts as the durable handoff.

## Task folder

```text
.ai/task/<TRACKER-KEY>/
  TICKET.md      # snapshot from Linear (Linear wins on divergence)
  TASK.md        # refined intent
  PLAN.md        # group graph — authority for progress
  STATUS.md      # derived: phase, current group, n/m, blocked
  DECISIONS.md   # append-only Decision blocks
  SCOUT.md       # persisted scout map
  review/        # address-review rounds
```

`PLAN.md` wins. Then rewrite `STATUS.md` and `task/_index.md`.

## Docs

Start flat: `docs/<topic>.md`. When a topic needs a second document, make `docs/<topic>/` with `_index.md`. Agents read the root `_index.md` first. They open a topic only when the row matches.

Default at close-out: write nothing. Promote only when a future agent needs the fact without the task folder.

## What does not belong here

| Put it elsewhere | Why |
| --- | --- |
| Tracker issues and PRDs after push | Linear |
| Binding MUST/MUST NOT for day-to-day code | `.ai/adr/` plus a Cursor rule |
| Chat transcripts, memory dumps | Nowhere. They rot |

Follow `skills/ai-dir/SKILL.md` for bootstrap and writes.
