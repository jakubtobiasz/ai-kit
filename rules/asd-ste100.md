---
name: asd-ste100
description: English STE everywhere
cursor:
    alwaysApply: false
aibits:
    deps:
        ~/skills/asd-ste100
---

# ASD-STE100 Simplified Technical English

Write English that another agent can parse without a human to ask. Use the **asd-ste100** skill for full rewrite rules, process, and output format.

This rule does **not** reproduce the ASD dictionary. Apply the structural rules with confidence. Treat lexical rules as a direction of travel. See the skill's Source and Scope section when you need more detail.

## HARD: language

**English is the only language for durable work.** Use ASD-STE100 on that English. This is not optional.

The language the human uses in chat is **only** for the live conversation between the human and you. Do not copy it into anything that lives in version control, a tracker, or a product surface.

Match the human's chat language. The chat language never changes the language of artifacts.

**Always English STE (never mixed with another language):**

- Identifiers, strings, and comments you add
- Tests, fixtures, log lines, error messages, user-visible copy you add
- Commits, branch names, and published change title and body
- Issue tracker items, comments, labels, project docs
- Skills, rules, and agent instructions in this repo
- Subagent prompts and status reports

**Never:**

- Translate a non-English chat request into a non-English ticket, commit, or published change
- Leave non-English text in a string, comment, or commit because the human used it in chat
- Mix languages in one artifact
- Ask whether to use English. Use English.

A non-English phrase in the human's message is input. Restate it in English STE in the artifact.

## Modes

**Strict** — procedures, error messages, tool descriptions, inter-agent prompts, skill bodies, always-applied rules, comments, log lines, user-visible error strings, tracker issues. Apply every structural rule. Cap instruction sentences at 20 words. Cap description sentences at 25 words.

**STE-flavored** — published change descriptions and explanatory prose. Apply structural rules. Do not lock one word per concept as if the dictionary were in this repo.

Pick Strict when a wrong reading has a cost. Pick STE-flavored for explanatory English prose. If unsure, pick Strict.

## Where it applies

| Surface | Mode | Notes |
| --- | --- | --- |
| Skills and skill assets that instruct an agent | Strict | Keep paths, identifiers, and output contracts unchanged |
| Rules | Strict | Same |
| Comments you add | Strict | Write a comment only when it adds value |
| User-visible errors, service messages, and log lines you add | Strict | Do not change existing product copy in the same change unless that change owns the string |
| Agent-to-agent prompts and status reports | Strict | One instruction per sentence |
| Issue tracker items and comments | Strict | Title and body in English STE even when the human asked in another language |
| Commits | Strict | English STE in the message body |
| Published change title and body | STE-flavored | English only |
| Chat with the human | Match the human | Chat is not an artifact |

## Do

- Use active voice. Name the actor.
- Give one instruction per sentence.
- Keep subject, verb, and article. Do not drop words to save space.
- Split a list of 3+ steps into a numbered or bullet list.
- Put leftover sentences after the list. Do not indent them under the last item.
- Keep hedges (`may`, `could`, `sometimes`). Do not promote a hedge to a fact.
- Reuse one name for one thing in a file. Do not rotate synonyms for the same entity.
- Keep paths, command names, and identifiers unchanged.

## Do not

- Use a semicolon.
- Use a phrasal verb when a single verb exists. Write `start`, not `spin up`. Write `remove`, not `take off`. Write `read`, not `dive into`.
- Stack 4+ nouns (`the handler that sets task-queue priority`, not `the agent task queue priority handler`).
- Use marketing adjectives (`seamless`, `robust`, `powerful`, `blazing-fast`).
- Apply STE to identifiers, symbol names, or generated schema text.
- Rewrite creative or marketing copy.
- Drop a safety condition, exception, or scope qualifier to shorten a sentence.
- Claim certified ASD-STE100 dictionary compliance. This repo does not ship the official dictionary.

## Comments

Clear names beat comments. When you write a comment, write it in Strict STE English. One idea per sentence. State the invariant or the trade-off. Do not narrate the next line.

## Communication

In English chat and published change text, prefer short active sentences. Lead with the fact. Then give the reason. Do not stack hedges. Do not open with a negation.

When you rewrite existing English on request, follow the skill process. Default output is the rewritten text alone, unless this task asked you to edit files in place.
