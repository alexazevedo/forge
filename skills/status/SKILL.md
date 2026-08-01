---
name: status
description: Use to see forge run state or resume after a break — prints the ticket board from STATE.md and ticket statuses. Triggers on /forge:status or "where were we".
---

# forge:status — board + resume

Read ONLY: PRODUCT.md, STATE.md, and `grep -r "^status:" tickets/`.
Do not read ticket bodies, source files, or chat history to answer.

Print the board:

```
forge: <product> [mode]
done:    01 slug, 02 slug
doing:   03 slug
blocked: 05 slug — <first line of its diagnosis, from STATE.md Blockers>
next:    04 slug (sonnet), 06 slug (haiku)
```

Then offer exactly one action: "continue" → re-enter forge:run (which
reuses the execution choice recorded in STATE.md Decisions — no re-asking).

If STATE.md is missing or disagrees with ticket frontmatter, trust the
ticket frontmatter, rewrite STATE.md, and say so in one line.
