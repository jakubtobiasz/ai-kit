# Grill Me — finding examples

Use these as a quality model. Imitate the finding shape. Do not copy the domain.

## Bad → good

### "Vague" is not a finding

Bad:

> **[testability] AC is vague** — the import ticket — needs more detail.

Good:

> **[testability] AC is not falsifiable** — `tickets/import.md` / Acceptance criteria — "the import works" does not define row count or malformed-row behavior. A tester cannot fail the ticket. Suggested fix: state the expected row count and the outcome for a malformed row.

### Completeness without a hole

Bad:

> **[completeness] Missing edge cases** — the PRD — should cover more cases.

Good:

> **[completeness] Partial import has no ticket** — `PRD.md` / Happy path — "users import a CSV of products" never states what happens when row 40 of 100 fails. Implementers will ship all-or-nothing or silent skip. Question: does a failed row block the batch, or land in an error report?

### Clarity nit that does not stall

Bad:

> **[clarity] Title could be nicer** — `tickets/01.md` / title — "Import" is short.

Drop it. A short title that still names the work is not a stall.

Good (only when a stranger would guess):

> **[clarity] "Notify" has no channel** — `tickets/02.md` / AC — "Then the merchant is notified" does not say email, in-app, or webhook. A stranger will pick one. Question: which channel, and is it in this ticket?

## Sample ranked list

Input (compressed): a `.ai/idea/csv-import/` directory. `PRD.md` promises CSV product import for staff. Tickets cover a happy-path upload. AC says "the import works". `TECH_PRD.md` says the job is async. The PRD says the user waits on the same page.

Output:

> **[contradictions] Sync page wait vs async job** — `PRD.md` / Flow vs `TECH_PRD.md` / Processing — PRD says "the user waits on the page until import finishes". TECH_PRD says "process in a background job". Two valid readings. Implementers will split. Question: is the UI blocking, or does it poll / notify?
>
> **[completeness] Malformed row has no behavior** — `tickets/01-upload.md` / Acceptance criteria — the happy path never names a bad row. The sprint can ship a silent skip. Suggested fix: add a ticket or AC for rejected rows and the staff-visible error.
>
> **[testability] AC is not falsifiable** — `tickets/01-upload.md` / Acceptance criteria — "the import works" has no row count, no duration bound, no visible success text. A tester cannot fail the ticket. Suggested fix: "Given a 10-row valid CSV, when staff import it, then 10 products exist and staff see '10 products imported'."
>
> **[scope] "Any CSV" conceals mapping work** — `PRD.md` / In scope — "staff can import any CSV" implies arbitrary columns. No mapping ticket exists. Suggested fix: bound the columns (SKU, name, price) or add a mapping ticket and mark the rest out of scope.
>
> **[dependencies] Preview ticket assumes an unbuilt parser** — `tickets/02-preview.md` / Depends on — preview has no `depends on`. It needs the parser from `01-upload.md`. Suggested fix: add `depends on: 01-upload` or merge parse into preview.
>
> **[assumptions] File size is unbounded** — `TECH_PRD.md` / Input — "accept a CSV upload" states no size or row cap. A large file will timeout or exhaust memory. Question: what is the max size and max row count, and what happens above it?
>
> **[feasibility] Optimistic "same request" parse** — `tickets/01-upload.md` / Estimate — sized as small, while TECH_PRD requires a job queue the repo may not have. Most likely to fail. Question: does a queue already exist, or is that hidden work?
>
> **[clarity] solid** — roles, terms, and file names are consistent enough for a stranger to start.

## Top 3 must-fix

1. [contradictions] Sync page wait vs async job
2. [completeness] Malformed row has no behavior
3. [testability] AC is not falsifiable

## Solid angle

When an angle has no weakness:

> **[feasibility] solid** — no finding.

Do not add a mild nit to avoid a solid line.

## Refuse a rewrite

Input: "grill `PRD.md` and rewrite the weak parts".

Output: the ranked findings list only. Do not edit `PRD.md`. Do not paste a rewritten PRD.
