---
name: idea
description: Use when the user brings a raw product idea to forge — grills it into a right-sized PRODUCT.md. Triggers on /forge:idea, a new product idea, or "let's build X" with no spec yet.
---

# forge:idea — grill the raw idea

Turn a raw idea into PRODUCT.md. Nothing else: no tickets, no code, no
architecture debate.

## The dial (question 1, always)

First question, every time: which product class? Recommend one based on what
they said, so "yes" is a valid answer.

| | prototype | mvp | production |
|---|---|---|---|
| grill questions (max) | 4 | 8 | as needed |
| PRODUCT.md body (max) | 250 words | 600 words | 1 page + ADRs if warranted |
| human approvals (whole pipeline) | 1 | 2 | 3 |
| tests | smoke only (build runs, happy path executes) | core logic only | full discipline + reviewer pass |
| loop attempts per ticket | 2 | 3 | 4 |
| permitted shortcuts | hardcode config, TODOs, zero abstractions "for later" | some hardcoding, documented | none |

Question 2: greenfield or existing repo? If the cwd already answers this,
don't ask — confirm in passing.

## How to grill

- ONE question per message. Multiple-choice preferred. ALWAYS include your
  recommended answer so the user can just say "yes".
- If the codebase can answer a question, explore instead of asking.
- Count your questions. Stop at the mode's cap — or earlier, the moment you
  can state the idea sharply. The cap is a ceiling, not a quota.
- Grill on: who it's for, the one job it must do, what's explicitly out,
  what "done" looks like. Never grill on implementation detail — that is
  forge:plan's job.

## Output: PRODUCT.md

Write from templates/PRODUCT.md at the repo root. Frontmatter: `mode`,
`routing_profile` (`cloud` unless the user themselves brought up local
models — never enable `cloud+local` silently), `parallel_default`. Body:
what/why, primary user story, out-of-scope list, success demo ("done when I
can ___", one sentence). Respect the mode's word cap.

## Gate

- production: show PRODUCT.md, get explicit approval. (Approval 1 of 3.)
- prototype / mvp: NO approval here — say "next: /forge:plan". Spec and
  tickets are approved together there. One interruption, not two.
