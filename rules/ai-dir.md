---
description: >-
  .ai directory contract. Linear owns tickets. idea/ is staging.
  task/ docs/ adr/ board-voting/ are the durable buckets. Always apply
  when reading or writing under .ai/ or when pushing issues.
cursor:
    alwaysApply: true
aibits:
    deps:
        ~/skills/ai-dir
        ~/skills/asd-ste100
---

# .ai directory contract

## Scope

This rule governs files under `.ai/` in a consuming repository. It also governs how tickets relate to Linear (or another issue tracker).

## Principle

Linear owns tickets. The repository owns execution state and lasting notes. Staging drafts do not survive a successful push.

## Rules

- MUST treat these four buckets as durable: `.ai/task/`, `.ai/docs/`, `.ai/adr/`, `.ai/board-voting/`.
- MUST treat `.ai/idea/<slug>/` as a draft window only. MUST NOT commit it. MUST delete the slug after every issue, link, and PRD attachment succeeds.
- MUST use a real tracker key as `.ai/task/<task-id>/` after push. MUST NOT keep a kebab id for work that already has an issue.
- MUST treat `PLAN.md` as group-level authority. `STATUS.md` and `.ai/task/_index.md` are derived. If they disagree, PLAN wins. Then rewrite the derived files.
- MUST start docs at `.ai/docs/_index.md`. MUST write one topic as one file. MUST split a topic into a directory with its own `_index.md` when a second document appears.
- MUST NOT write a docs file per ticket. Default at task close-out is to write nothing under `docs/`.
- MUST put binding architecture in `.ai/adr/`. MUST put living notes in `.ai/docs/`. MUST put advisory board votes in `.ai/board-voting/`.
- MUST NOT treat staging markdown as the durable handoff when no tracker MCP is connected. Stop. Connect a tracker. Then push.
- MUST follow `skills/ai-dir/SKILL.md` for bootstrap, STATUS writes, docs promotion, and staging delete.

## Exceptions

- Raw-intent develop with no tracker issue MAY use a kebab `task-id` until an issue exists.
- `.ai/board-voting/` keeps `INDEX.md` as its catalog name. Other catalogs use `_index.md`.
- In-task `review/INDEX.md` stays the round list. It is not a bucket catalog.

## Examples

### Prefer

Approved tickets → push to Linear → delete `.ai/idea/catalog-import/` → `/develop ENG-45`.

### Avoid

Keep `.ai/idea/catalog-import/` in git and deliver the story from those files after the issues exist.

## References

- `skills/ai-dir/SKILL.md` — bootstrap, STATUS, docs promotion, staging delete
- `skills/push-issues/SKILL.md` — create Linear issues, then delete staging
- `skills/develop/SKILL.md` — resume from `.ai/task/<key>/`
