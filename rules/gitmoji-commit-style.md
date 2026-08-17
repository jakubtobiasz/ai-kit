---
description: Commit messages and history hygiene
cursor:
    alwaysApply: true
aibits:
    deps:
        ~/skills/asd-ste100
---

# Committing

## Messages

- Commit messages must be English ASD-STE100 skill after the gitmoji.
- Commit messages must be business-oriented
  - **GOOD** Add handling a remote config file
  - **GOOD** Handle a config file upload
  - **BAD** Create an organization entity
- Commit messages must follow gitmoji convention
  - **GOOD** ♻️ Extract common logic from the XYZ service
  - **BAD** Extract common logic from the XYZ service
- Commits should be a single-responsible
  - **GOOD** ♻️ Extract common logic from the XYZ service
  - **BAD** ♻️ Extract common logic from the XYZ service and configure entity
- Commits with multiple responsibilities should be split into more than one

## History hygiene (before push / PR)

Prefer a **small number of coherent commits** over many micro-commits for the same change set.

- **Do not** leave a trail of WIP / fixup / “note”, “tweak link”, “drop artifact”, or “clarify wording” commits that belong to the same concern.
- **Before** opening or updating a PR, squash or rewrite that noise into the fewest single-responsibility commits that still match the rules above (typically 1–3 for a docs/ADR/skill change; one commit per PLAN group for implementation work).
- A branch whose commit count is large relative to the file count (e.g. ~10 commits for ~10 files of the same initiative) is a smell — squash before you push or force-push with lease on the feature branch.
- Mid-work local commits are fine; the **published** branch history must be reviewable.

```text
# BAD — published history
📝 Convene board votes as architect seats
📝 Clarify architect seats for implementation consensus
📝 Persist BV-0002 …
📝 Accept ADR-0004 …
📝 Link ADR-0004 from root AGENTS …
📝 Mention ADR-0004 in ADR index …
📝 Point BV-0002 at landed ADR-0004 …
📝 Drop idea artifacts …
📝 Note idea markdown is WIP …

# GOOD — same end state, published history
📝 Convene board votes as architect seats
📝 Adopt rich domain models in core
```
