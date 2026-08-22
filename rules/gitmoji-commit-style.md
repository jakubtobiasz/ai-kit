---
description: >-
  Gitmoji-prefixed STE commit subjects and single-responsibility published
  history. Always apply when creating or rewriting commits or preparing a
  push or pull request.
cursor:
    alwaysApply: true
aibits:
    deps:
        ~/skills/asd-ste100
---

# Gitmoji commit style

## Scope

This rule governs commit subjects and published branch history on push and pull request.

It is global unless a more specific rule overrides it.

Local mid-work commits MAY ignore message style until publish.

## Principle

A published commit starts with a gitmoji. Then it states one business change in English STE. Published history has no WIP or fixup noise.

## Priority

This rule owns the gitmoji prefix, business wording, one responsibility per commit, and published-history shape.

English STE for the words after the gitmoji comes from `rules/asd-ste100.md` and the `asd-ste100` skill.

This rule does not override explicit user-supplied commit text when the user gave the full message.

## Rules

- MUST start the commit subject with a gitmoji, then a space, then English STE.
- MUST follow [gitmoji](https://gitmoji.dev) for the prefix.
- MUST describe the user-visible or business change.
- MUST NOT name a class, table, or entity unless that artifact is the user-visible change.
- MUST give each published commit one responsibility.
- MUST NOT join unrelated changes with "and".
- MUST publish the fewest single-responsibility commits that cover the change.
- MUST NOT publish WIP, fixup, or "tweak" / "note" / "drop" commits for the same concern.
- SHOULD NOT publish about one commit per file for one initiative.
- MUST write the English after the gitmoji with the `asd-ste100` skill. Strict mode.

## Exceptions

- Local mid-work commits MAY use any subject until publish.
- Merge and revert commits MAY keep the generated subject.
- If the user supplied the full commit message, MUST use that text.

## Examples

### Prefer

```text
♻️ Extract shared validation so checkout rejects invalid carts
```

```text
🎉 Add CSV product import so staff can load a catalog
```

### Avoid

```text
Extract common logic from the XYZ service
```

No gitmoji.

```text
♻️ Extract shared validation and configure the organization entity
```

Two responsibilities. Names an entity that is not the user-visible change.

```text
📝 Note idea markdown is WIP
📝 Tweak link
📝 Drop artifact
```

Published trail for one concern.

### Exception

A local `wip` commit on an unpublished branch is allowed. Rewrite it before push or pull request.

## References

- [gitmoji](https://gitmoji.dev) — allowed prefixes
- `rules/asd-ste100.md` — English STE on durable artifacts
- `~/skills/asd-ste100` — how to write the English after the gitmoji
