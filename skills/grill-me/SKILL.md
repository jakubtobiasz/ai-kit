---
name: grill-me
description: >-
  Adversarially stress-test a written PM artifact — tickets, a PRD.md, a
  TECH_PRD.md, a spec, a plan — before it is locked. Use when someone wants a
  hard, skeptical review that searches for weak assumptions, gaps, missing
  pieces, vague or untestable acceptance criteria, hidden dependencies, and
  scope ambiguity. Triggers include "grill these tickets", "grill my PRD",
  "grill the TECH_PRD", "poke holes in this PRD", "stress-test this spec",
  "what am I missing on this ticket", "red-team this plan"
aibits:
  deps:
    - ~/skills/writing-gherkin-scenarios
    - ~/skills/asd-ste100
---

# Grill Me

## Purpose

Find failures in a written PM artifact before implementation. Do not ask the author. Do not change the artifact.

Grill means search the text for weaknesses and return findings. Attack the work. Never attack the author.

## Activation

### Use when

Use this skill when:

- a human asks to grill tickets, a PRD, a TECH_PRD, a spec, or a plan
- a human asks to poke holes in a PRD, stress-test a spec, red-team a plan, or find what a ticket or spec is missing
- `product-manager`, `technical-product-manager`, or `delivery-planner` invokes this skill in a subagent
- a written PM artifact must be tested for weaknesses before the team accepts it

### Do not use when

Do not use this skill when:

- the task is to rewrite or fix the artifact
- the task is a code review
- the task is a security review of a running system
- the task is an ADR or architecture review
- the target is not a written PM artifact
- the task is to interview a live user
- no written artifact exists yet

If the caller asks for a grill and a rewrite, grill only. Return findings. Do not rewrite.

## Context

The target is one file, or a whole staging directory `.ai/idea/<slug>/`. In a directory, the artifact includes every ticket, `PRD.md`, `TECH_PRD.md`, parent file, and any `references/`. Grill during the draft window. After push, the Linear issue (and its PRD attachment) is the source of truth.

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
11. Rank findings from most costly to least costly.
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

Check every angle. A finding cites a file, quotes a line, and names the implementation failure. "Vague" is not a finding.

- **Assumptions** — a stated fact that was never verified
- **Completeness / gaps** — missing state, role, edge, non-functional need, or ticket
- **Testability** — an acceptance criterion that cannot be failed
- **Scope** — in vs out is unclear, or one line hides a large body of work
- **Dependencies & order** — unstated coupling, wrong sequence, or a missing `depends on`
- **Feasibility & risk** — the risky part is unnamed or sized as easy
- **Clarity** — a new reader cannot execute this without the author
- **Contradictions** — two statements that cannot both be true

When you check completeness, walk these in order:

1. States (empty, error, loading, partial)
2. Roles
3. Edges
4. Non-functionals the feature type requires
5. Missing tickets or steps

Read `references/attack-angles.md` for probe questions.

## Constraints

- MUST read the full target before you report.
- MUST check every attack angle.
- MUST cite file, section, and a quoted line on every finding.
- MUST rank most costly first.
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

A concise, ranked list. Most costly first. No praise. No filler. No rewrite. Each finding:

> **[angle] one-line weakness** — _where_ (file / section) — why it fails during implementation — suggested fix or the question that must be answered.

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
- [ ] Findings are ranked most costly first.
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

### Gherkin AC

Input: a scenario with "When I click `#submit`" and a second `When` that places an order.

Expected: flag **[testability]**. The step is imperative. The scenario covers two behaviors. Quote the lines. Do not rewrite the AC.

### Source code attached

Input: the caller points at application source instead of a PRD, ticket, spec, or plan.

Expected: refuse. This skill is not a code review.

## Failure Modes

- Target path missing and not inferable → stop. Name the required input. Do not guess.
- Listed file unreadable → completeness finding. Continue with files you read.
- Caller asks to rewrite → grill only. Do not edit.
- Caller asks to interview the author → grill the text. Put needed questions inside findings.
- Artifact is empty → one completeness finding. Stop.
- Artifact is source code → refuse. This skill is not a code review.
- You want to pad the list → stop adding. A short list is correct.

## References

- `references/attack-angles.md` — probe questions per angle. Read once per run after you read the artifact.
- `references/examples.md` — good vs bad findings and a full sample list. Read when the finding bar is unclear.
- `writing-gherkin-scenarios` — Gherkin AC bar. Read when the artifact contains Gherkin and the skill is available.
