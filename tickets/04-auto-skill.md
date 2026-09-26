---
id: 04
status: done
model: opus
depends_on: [01]
max_attempts: 2
files: [skills/auto/SKILL.md, docs/discovery-auto-owner.md]
---
## Goal
New `/forge:auto [mode] "<vision>"` skill: sets `owner: agent`, chains idea → plan → run, ends with REPORT.md.

## Context
`docs/discovery-auto-owner.md` is read-only design context. Create `skills/auto/SKILL.md`, frontmatter `name: auto` and description: "Use when the user wants forge to run end-to-end with an owner-proxy agent answering the grill — they decide only the product class. Triggers on /forge:auto, $forge:auto, 'auto mode', 'run it without asking me'." Body ≤55 lines, terse like the other skills:
1. Mode from the first argument (prototype | mvp | production). Missing → ask exactly ONE question with a recommendation; if the vision is also missing, ask for both in that same message. Nothing else is ever asked.
2. State once: no further prompts except production gates; headless runs need `--permission-mode auto`.
3. Invoke forge:idea with the vision; PRODUCT.md frontmatter gets `mode` and `owner: agent`. Then forge:plan, then forge:run. Include the compact gate table (prototype: owner approves all; mvp: owner approves spec+tickets, diff summary to report; production: owner drafts, human approves spec, plan, final).
4. REPORT.md at repo root: vision, mode, grill counts (AGREE / DIVERGE / ESCALATE), every DIVERGE row in full, ticket board, blocked tickets with diagnosis, how to run the result (last ticket's runtime probe), mvp diff summary. Last printed line: report path plus the counts.
5. Resume: PRODUCT.md with `owner: agent` and STATE.md present → re-enter forge:run via forge:status "continue", ask nothing.
6. Hard stops: production gates wait for the human; ESCALATE in mvp / production stops; never lower the mode.

Notes from ticket 01: double-quote the `description:` value in the frontmatter (validate does not catch YAML breakage). Owner output contract: `ANSWER:` / `TAG: AGREE|DIVERGE|ESCALATE` / `REASON:`; approval = `APPROVE` tagged AGREE or objections tagged DIVERGE. The grill log lives in PRODUCT.md `## Grill log` and is outside the body word cap.

## Checks (run in order — cheapest first)
1. `claude plugin validate ~/dev/forge --strict`
2. `grep -q '^name: auto' skills/auto/SKILL.md && grep -q 'REPORT.md' skills/auto/SKILL.md && grep -q 'owner: agent' skills/auto/SKILL.md`
3. `test $(wc -l < skills/auto/SKILL.md) -le 60 && test $(cat skills/*/SKILL.md | wc -l) -le 600`

## Done when
- `/forge:auto` is a valid plugin skill implementing the six points.

## Not included
- README; edits to any other skill.
