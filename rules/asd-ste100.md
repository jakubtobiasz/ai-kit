---
description: >-
  English STE on durable artifacts. Match the human only in live chat.
  Use when writing commits, tickets, skills, rules, comments, errors, or
  published change text.
cursor:
    alwaysApply: false
aibits:
    deps:
        ~/skills/asd-ste100
---

# ASD-STE100 Simplified Technical English

## Scope

This rule governs durable artifacts in this repository and in trackers.

Durable artifacts are files, commits, tickets, comments, labels, skills, rules, agent prompts, status reports, and published change text.

Live chat with the human is out of scope except for the chat-language rule below.

When this rule is loaded, English STE is mandatory for those artifacts. Frontmatter `alwaysApply: false` does not weaken the policy.

## Principle

Write English that another agent can parse without a human. Keep the human's chat language in chat only.

## Priority

This rule takes precedence over matching the human's language in artifacts.

This rule does not override:

- explicit security requirements
- higher-priority system instructions
- `gitmoji-commit-style` for the gitmoji prefix

If the user asks for a non-English artifact, MUST still write the artifact in English STE. MUST NOT ask which language to use.

STE applies to the English after a required prefix such as gitmoji. It does not apply to the prefix itself.

## Rules

- MUST write durable artifacts in English STE.
- MUST follow the `asd-ste100` skill for rewrite rules, process, and output format.
- MUST match the human's language in live chat only.
- MUST NOT copy the human's chat language into a durable artifact.
- MUST NOT mix languages in one artifact.
- MUST NOT ask whether to use English.
- MUST pick the mode from the surface table. If unsure, MUST pick Strict.
- MUST keep paths, command names, and identifiers unchanged.
- MUST keep hedges (`may`, `could`, `sometimes`). MUST NOT promote a hedge to a fact.
- MUST NOT write a comment that only restates the next line.
- A comment MUST state an invariant or a trade-off.
- MUST NOT change existing product copy unless this change owns that string.
- MUST NOT claim certified ASD-STE100 dictionary compliance.
- MUST NOT reproduce ASD's official dictionary.

### Surfaces

| Surface | Mode |
| --- | --- |
| Skills and skill assets that instruct an agent | Strict |
| Rules | Strict |
| Comments you add | Strict |
| User-visible errors, service messages, and log lines you add | Strict |
| Agent-to-agent prompts and status reports | Strict |
| Issue tracker items and comments | Strict |
| Commit message English after the gitmoji | Strict |
| Published change title and body | STE-flavored |
| Chat with the human | Match the human |

A published change is a pull request or other reviewable change description sent outside this chat.

## Exceptions

- Live chat MAY use the human's language.
- Identifiers, symbol names, and generated schema text MAY stay as written by the system or the codebase.
- Creative or marketing copy is out of scope. MUST NOT apply STE to it.
- Existing product strings this change does not own MAY stay unchanged.
- A required prefix such as gitmoji MAY stand before the English STE subject.

## Examples

### Prefer

Human: "dodaj bilet na import CSV"

Ticket title: `Add CSV product import`

### Avoid

Human: "dodaj bilet na import CSV"

Ticket title: `Dodaj bilet na import CSV`

### Exception

A French error string that this change does not own stays in French.

## References

- `~/skills/asd-ste100` — rewrite rules, process, and output format
- `skills/asd-ste100/references/writing-rules.md` — Issue 9, dictionary limits, redistribution
- `rules/gitmoji-commit-style.md` — gitmoji prefix and published-history shape
