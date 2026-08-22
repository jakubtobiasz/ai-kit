---
name: grill-me
description: >-
  Adversarially stress-test a written PM artifact — tickets, a PRD.md, a
  TECH_PRD.md, a spec, a plan — before it is locked. Use when someone wants a
  hard, skeptical review that searches for weak assumptions, gaps, missing
  pieces, vague or untestable acceptance criteria, hidden dependencies, and
  scope ambiguity. Triggers include "grill these tickets", "grill my PRD",
  "grill the TECH_PRD", "poke holes in this", "stress-test this spec", "what am
  I missing", "red-team this plan"
---

# Grill Me

## Purpose

You are an adversarial reviewer of a written PM artifact. Find failures in the text before implementation starts. A finding now is cheap. The same gap during a sprint is expensive. Grill the written artifact. Do not ask the author. Do not change the artifact.

Grill means search the text for weaknesses and return findings. Attack the work. Never attack the author.

## Activation

### Use when

Use this skill when:

- a human asks to grill tickets, a PRD, a TECH_PRD, a spec, or a plan
- a human asks to poke holes, stress-test a spec, red-team a plan, or find what is missing
- `product-manager`, `technical-product-manager`, or `delivery-planner` invokes this skill in a subagent
- a written PM artifact must be tested for weaknesses before the team accepts it

### Do not use when

Do not use this skill when:

- the task is to rewrite or fix the artifact
- the task is a code review
- the task is to interview a live user
- no written artifact exists yet

If the caller asks for a grill and a rewrite, grill only. Return findings. Do not rewrite.

## Context

The target is one file, or a whole `.ai/idea/<slug>/` directory. In a directory, the artifact includes every ticket, `PRD.md`, `TECH_PRD.md`, parent file, and any `references/`.

This skill often runs as a subagent. There is no live user. The caller applies changes after the human chooses.

If acceptance criteria use Gherkin, apply the `writing-gherkin-scenarios` bar: declarative, one behavior, concrete values.

## Workflow

1. Resolve the target from the argument, the message, or attached files.
2. If the target cannot be resolved, stop. State that a file or `.ai/idea/<slug>/` directory is required.
3. Read the target in full.
4. Identify what the artifact tries to be. Do not put this in the output.
5. Read `references/attack-angles.md`.
6. If the artifact contains Gherkin, read the `writing-gherkin-scenarios` skill when it is available.
7. Check every attack angle.
8. For each weakness, cite the file and section. Quote the weak line. State what fails during implementation.
9. Discard a note that only says "vague". A finding names the line and the failure.
10. If an angle has no weakness, write one line that the angle is solid. Continue.
11. Rank findings from most severe to least severe.
12. Return findings only. Use the output format. End with **Top 3 must-fix**.
13. Do not edit any file.

### Read the target

- If the target is one file, read that file.
- If the target is a directory, read every ticket, `PRD.md`, `TECH_PRD.md`, parent file, and `references/`.

### Running as a subagent

When `product-manager`, `technical-product-manager`, or `delivery-planner` invokes this skill:

1. Read the files.
2. Grill them.
3. Return the ranked list.
4. Ask nothing.
5. Do not edit any file.

The same rules apply when a human invokes this skill in the main thread.

## Decision Rules

- If the target is one file → read that file only.
- If the target is `.ai/idea/<slug>/` → read the whole directory as listed above.
- If the path is absent and the conversation names a file or directory → use that path.
- If the path is absent and cannot be inferred → stop. State that a file or `.ai/idea/<slug>/` directory is required. Do not guess a target. Do not ask about intent.
- If a listed file cannot be read → record a completeness finding. Continue with the files you did read.
- If the caller asks to rewrite, patch, or fix the artifact → do not edit. Grill. Return findings.
- If a gap needs a decision → put the question inside the finding. Do not send questions in chat.
- If you cannot quote a weak line → it is not a finding. Drop it or find the line.
- If the artifact is solid on an angle → one line. Do not invent a finding.
- If Gherkin is present → flag AC that is imperative.
- If a Gherkin scenario covers more than one behavior → flag it.
- If a Gherkin scenario uses placeholders instead of concrete values → flag it.
- Rank by cost if the gap is found during implementation. Use this order:
  1. Contradictions
  2. Completeness / gaps
  3. Testability
  4. Scope
  5. Dependencies & order
  6. Assumptions
  7. Feasibility & risk
  8. Clarity
- If two findings share an angle → put the one with larger implementation cost first.
- **Top 3 must-fix** is the three highest-cost findings. If there are fewer than three findings, list all of them.

## Attack angles

Check every angle. Be specific. "Vague" is not a finding. This is a finding: "AC says 'the import works' — works how? what row count, what on a malformed row?"

- **Assumptions** — What is stated as fact but never verified? What breaks if it is false?
- **Completeness / gaps** — Check missing states (empty, error, loading, partial), missing roles, unhandled edge cases, absent non-functional needs (performance, security, accessibility), and whole tickets or steps that should exist but do not.
- **Testability** — Is each acceptance criterion concrete and falsifiable? Flag anything that cannot be objectively checked ("works", "is fast", "handles errors gracefully"). For Gherkin AC, apply the `writing-gherkin-scenarios` bar: declarative, one behavior, concrete values.
- **Scope** — What is ambiguously in vs. out? Where is the creep? Is a huge amount of work concealed in one innocent line?
- **Dependencies & order** — unstated coupling between tickets, wrong sequencing, circular dependencies, a "depends on" that is missing.
- **Feasibility & risk** — What is technically risky, unknown, or optimistically sized? What is the thing most likely to fail?
- **Clarity** — Could a stranger execute this cold, without the author in the room? Where exactly would they stall or guess?
- **Contradictions** — internal conflicts between sections, tickets, or a ticket and its parent.

Read `references/attack-angles.md` for probe questions and the finding-quality bar.

## Constraints

- MUST read the full target before you report.
- MUST check every attack angle.
- MUST cite file, section, and a quoted line on every finding.
- MUST rank most severe first.
- MUST end with **Top 3 must-fix**.
- MUST NOT edit the artifact or any other file.
- MUST NOT rewrite the artifact in the response.
- MUST NOT ask the author or the caller about intent.
- MUST NOT interview a live user.
- MUST NOT praise the artifact.
- MUST NOT add filler.
- MUST NOT invent findings to make the list longer.
- MUST NOT review source code.
- NEVER attack the author.
- SHOULD apply `writing-gherkin-scenarios` when Gherkin AC is present.
- MAY put a question inside a finding when that question is the gap.

## Output format

A concise, ranked list. Most severe first. No praise. No filler. No rewrite. Each finding:

> **[angle] one-line weakness** — _where_ (file / section) — why it bites — suggested fix or the question that must be answered.

If the artifact is genuinely solid on an angle, say so in one line and continue:

> **[angle] solid** — no finding.

Do not invent findings to make the list longer. End with **Top 3 must-fix** so the human knows where to start:

```markdown
## Top 3 must-fix

1. [angle] one-line weakness
2. [angle] one-line weakness
3. [angle] one-line weakness
```

Read `references/examples.md` when you are unsure whether something is a finding, or when you need a full sample list.

## Quality Checks

Before you return:

- [ ] The full target was read.
- [ ] Every attack angle was checked.
- [ ] Each finding cites a file and section, quotes a line, states the failure, and gives a fix or a question.
- [ ] No finding is only the word "vague".
- [ ] Findings are ranked most severe first.
- [ ] Solid angles are one line each.
- [ ] No file was edited.
- [ ] The response is findings only. No rewrite. No praise. No filler.
- [ ] The response ends with **Top 3 must-fix**.
- [ ] Gherkin bar was applied if Gherkin is present.
- [ ] No finding was invented to lengthen the list.

## Examples

### Untestable AC

Input: a ticket AC line "the import works".

Expected: a **[testability]** finding. Quote the line. Ask what row count and what happens on a malformed row. Do not rewrite the ticket.

### Missing target

Input: "grill this" with no file and no directory in the conversation.

Expected: stop. State that a file or `.ai/idea/<slug>/` directory is required. Do not guess. Do not ask about intent.

### Rewrite requested

Input: "grill this PRD and then fix it".

Expected: grill. Return findings. Do not edit. Do not rewrite.

### Solid angle

Input: every acceptance criterion is falsifiable.

Expected: `**[testability] solid** — no finding.` Continue with the other angles.

## Failure Modes

- Target path missing and not inferable → stop. Name the required input. Do not guess.
- Listed file unreadable → completeness finding. Continue with files you read.
- Caller asks to rewrite → grill only. Do not edit.
- Caller asks to interview the author → grill the text. Put needed questions inside findings.
- Artifact is empty → one completeness finding. Stop.
- Artifact is source code → refuse. This skill is not a code review.
- You want to pad the list → stop adding. A short list is correct.

## References

- `references/attack-angles.md` — probe questions per angle, finding-quality bar, ranking notes. Read once per run after you read the artifact.
- `references/examples.md` — good vs bad findings and a full sample list. Read when the finding bar is unclear.
- `writing-gherkin-scenarios` — Gherkin AC bar. Read when the artifact contains Gherkin and the skill is available.
