---
id: NN
status: todo            # todo | doing | done | blocked
model: sonnet           # haiku | sonnet | opus | local
depends_on: []
max_attempts: 2         # from mode: prototype 2 | mvp 3 | production 4
files: []               # expected touch set; disjoint across parallel tickets
---
## Goal
One sentence.

## Context
≤150 words. Everything the agent needs that isn't in the files. No links to
chat history.

## Checks (run in order — cheapest first)
1. `<lint command>`
2. `<build command>`
3. `<test command>`     <!-- mvp and up only -->
4. `<runtime probe, e.g. curl -s localhost:3000/health | grep ok>`

## Done when
- <observable behavior, one or two bullets>

## Not included
- <explicit scope boundary so the agent doesn't gold-plate>
