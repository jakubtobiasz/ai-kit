# <task-id> / <R<n>> — review feedback

> Round: **R<n>**.
> PR: <url or "pasted">
> Branch: <headRefName or current>
> Platform: github | origin | linear | pasted

Record a reply id for every remote comment. §6 cannot reply without it.

## Items

<!-- ids F1… are per-round. Do not continue numbering from prior rounds. -->

### F1

- **Platform:** github | origin | linear | pasted
- **Kind:** inline | top-level
- **Comment id:** <GitHub REST integer | Origin `cmt_…` | Linear UUID | n/a>
- **Thread id:** <GitHub GraphQL `PRRT_…` | Origin `cth_…` | Linear top-level UUID | same as Comment id | n/a>
- **Path:** `<file>:<line>` or n/a
- **Author:** <login>
- **Body:**

```
<verbatim comment text>
```

### F2

- **Platform:** …
- **Kind:** …
- **Comment id:** …
- **Thread id:** …
- **Path:** …
- **Author:** …
- **Body:**

```
<verbatim comment text>
```
