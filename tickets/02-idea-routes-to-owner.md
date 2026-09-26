---
id: 02
status: done
model: sonnet
depends_on: [01]
max_attempts: 2
files: [skills/idea/SKILL.md, docs/discovery-auto-owner.md]
---
## Goal
forge:idea routes every grill question to the owner agent when `owner: agent` is set, and records a grill log in PRODUCT.md.

## Context
`docs/discovery-auto-owner.md` is read-only design context. Add one section `## Owner proxy (owner: agent)` of ≤12 lines to `skills/idea/SKILL.md`. Semantics: when `owner: agent` is set (PRODUCT.md frontmatter written by forge:auto, or stated in the invoking prompt), ask the user nothing. For each grill question dispatch the `forge:owner` subagent (Agent tool; on Codex inline the body of `agents/owner.md` as role instructions) with: vision, `~/.forge/owner.md` if present, PRODUCT.md so far, grill log so far, the question with your recommended answer, and the mode. Parse `ANSWER` / `TAG` / `REASON`; append a row to `## Grill log` in PRODUCT.md. The mode is never asked of the owner. ESCALATE: prototype → take the smallest-scope option and log it; mvp / production → stop, write STATE.md Blockers, tell the user. Gate: prototype / mvp unchanged ("next: /forge:plan"); production still shows PRODUCT.md and waits for the human. Question caps per mode still apply.

Notes from ticket 01: the `## Grill log` table sits outside the body word cap — do not count it. On ESCALATE the owner's ANSWER already carries the smallest-scope option, so prototype just uses it. YAML frontmatter descriptions containing `: ` must be double-quoted (validate does not catch this).

## Checks (run in order — cheapest first)
1. `claude plugin validate ~/dev/forge --strict`
2. `grep -q 'owner: agent' skills/idea/SKILL.md && grep -q 'Grill log' skills/idea/SKILL.md && grep -q 'forge:owner' skills/idea/SKILL.md`
3. `test $(wc -l < skills/idea/SKILL.md) -le 75`

## Done when
- The section exists and the three checks pass.

## Not included
- plan / run / status / auto skills; README.
