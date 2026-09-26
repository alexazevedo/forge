---
id: 03
status: done
model: sonnet
depends_on: [01]
max_attempts: 2
files: [skills/plan/SKILL.md, skills/run/SKILL.md, skills/status/SKILL.md, docs/discovery-auto-owner.md]
---
## Goal
plan, run and status honor `owner: agent` gate authority by mode.

## Context
`docs/discovery-auto-owner.md` is read-only design context. Each addition ≤8 lines, wording terse like the rest of the file.

plan, in "Output + gate": under `owner: agent` — prototype and mvp: dispatch `forge:owner` once with PRODUCT.md plus the ticket summary asking "APPROVE or list blocking objections"; APPROVE → proceed; objections → revise tickets once, then proceed. production: print the summary, say approval is pending, stop (approval 2 of 3 stays human).

run: the parallel-vs-sequential question — under `owner: agent` answer from `parallel_default`, never ask, record under STATE.md Decisions. Finish under `owner: agent`: prototype → no gate; mvp → write the diff summary into `REPORT.md` under "Final review" instead of waiting; production → reviewer pass as today, then stop and say final review is pending.

status: header line becomes `forge: <product> [mode] [owner: human|agent]`; when PRODUCT.md has a grill log, append a line `grill: N answers, D diverge, E escalate`.

Notes from ticket 01: the owner answers an approval request with `ANSWER: APPROVE` tagged AGREE, or a ≤5-line objection list tagged DIVERGE — parse it that way. YAML frontmatter descriptions containing `: ` must be double-quoted (validate does not catch this).

## Checks (run in order — cheapest first)
1. `claude plugin validate ~/dev/forge --strict`
2. `grep -q 'owner: agent' skills/plan/SKILL.md && grep -q 'owner: agent' skills/run/SKILL.md && grep -q 'owner' skills/status/SKILL.md`
3. `test $(cat skills/*/SKILL.md | wc -l) -le 600`

## Done when
- The three skills carry the owner-mode behavior above and checks pass.

## Not included
- idea / auto skills; README.
