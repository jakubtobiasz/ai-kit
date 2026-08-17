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

Write English that another agent can parse without a human to ask. Use [`.cursor/skills/asd-ste100/SKILL.md`](../skills/asd-ste100/SKILL.md). That skill is a vendor copy of [danyuchn/asd-ste100-skill](https://github.com/danyuchn/asd-ste100-skill).

This rule does **not** reproduce the ASD dictionary. Apply the structural rules with confidence. Treat lexical rules as a direction of travel. See the skill's Source and Scope.

## HARD: language

**English is the only language for durable work.** Use ASD-STE100 on that English. This is not optional.

Polish (or any other language the human uses in chat) is **only** for the live conversation between the human and you. Do not copy it into anything that lives in git, a tracker, or a product surface.

If the human writes Polish, reply in Polish. If the human writes English, reply in English STE-flavored. The chat language never changes the language of artifacts.

**Always English STE (never Polish, never mixed):**

- Source code identifiers, strings you add, comments, TSDoc
- Tests, fixtures, log lines, error messages, UI copy you add
- Git commits (words after gitmoji), branch names, PR title and body
- Linear, Jira, GitHub issues, comments, labels, project docs
- `.ai/` tickets, PRDs, ADRs, board votes, PLAN.md, TASK.md
- `.cursor/` skills, rules, agents (except this sentence that names Polish)
- Subagent prompts, `NEEDS:` blocks, tracker attachments

**Never:**

- Translate a Polish chat request into a Polish ticket, commit, or PR
- Leave Polish in a string, comment, or commit because the human used it in chat
- Mix Polish and English in one artifact
- Ask whether to use English. Use English.

A Polish phrase in the human's message is input. Restate it in English STE in the artifact.

## Modes

**Strict** — procedures, error messages, tool descriptions, inter-agent prompts, skill bodies, always-applied rules, comments, TSDoc, log lines, user-visible error strings, tracker issues. Apply every structural rule. Cap instruction sentences at 20 words. Cap description sentences at 25 words.

**STE-flavored** — PR bodies, README prose, English chat replies to a human. Apply structural rules. Do not lock one word per concept as if the dictionary were in this repo.

Pick Strict when a wrong reading has a cost. Pick STE-flavored for explanatory English prose. If unsure, pick Strict.

## Where it applies

| Surface | Mode | Notes |
| --- | --- | --- |
| `.cursor/skills/**/SKILL.md` and skill assets that instruct an agent | Strict | Keep paths, identifiers, output contracts, mermaid, and code fences |
| `.cursor/rules/**` | Strict | Same |
| `.cursor/agents/**` | Strict | Same |
| Code comments and TSDoc | Strict | Write a comment only when [commenting.mdc](./code-style/commenting.mdc) already permits one |
| User-visible errors, API messages, log lines you add | Strict | Do not change existing product copy in the same change unless that change owns the string |
| Agent-to-agent prompts, `NEEDS:` blocks, status reports | Strict | One instruction per sentence |
| Linear / Jira / GitHub issues and comments | Strict | Title and body in English STE even when the human asked in Polish |
| Git commits after gitmoji | Strict | See [commiting-rules.mdc](./commiting-rules.mdc) |
| PR title and body | STE-flavored | English only |
| Chat with the human | Match the human | Polish if they wrote Polish. English STE-flavored if they wrote English. Chat is not an artifact |
| Gherkin in tickets | English domain dialect | Follow `writing-gherkin-scenarios`. Still English. Not Polish |

## Do

- Use active voice. Name the actor.
- Give one instruction per sentence.
- Keep subject, verb, and article. Do not drop words to save space.
- Split a list of 3+ steps into a numbered or bullet list.
- Put leftover sentences after the list. Do not indent them under the last item.
- Keep hedges (`may`, `could`, `sometimes`). Do not promote a hedge to a fact.
- Reuse one name for one thing in a file (`Organization`, not a mix of org / tenant / customer for the same entity).
- Keep Atlas domain terms: `Organization`, `User`, `Team`, `TeamMembership`, `Node`, `Project`, `Deployment`, `spawn`, `dispatch`, `Task`, `subagent`, `scout`, `architect`, `orchestrator`.
- Keep file paths, command names, HTTP paths, and code identifiers unchanged.

## Do not

- Use a semicolon.
- Use a phrasal verb when a single verb exists. Write `start`, not `spin up`. Write `remove`, not `take off`. Write `read`, not `dive into`.
- Stack 4+ nouns (`the handler that sets task-queue priority`, not `the agent task queue priority handler`).
- Use marketing adjectives (`seamless`, `robust`, `powerful`, `blazing-fast`).
- Apply STE to identifiers, type names, or generated OpenAPI text.
- Rewrite creative or marketing copy.
- Drop a safety condition, exception, or scope qualifier to shorten a sentence.
- Claim certified ASD-STE100 dictionary compliance. This repo does not ship the official dictionary.

## Comments and code

Clear names beat comments. When you write a comment, write it in Strict STE English. One idea per sentence. State the invariant or the trade-off. Do not narrate the next line.

## Communication

In English chat and PR text, prefer short active sentences. Lead with the fact. Then give the reason. Do not stack hedges. Do not open with a negation.

When you rewrite existing English on request, follow the skill process. Default output is the rewritten text alone, unless this task asked you to edit files in place.

