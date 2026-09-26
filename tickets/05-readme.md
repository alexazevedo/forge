---
id: 05
status: done
model: haiku
depends_on: [04]
max_attempts: 2
files: [README.md]
---
## Goal
README documents forge:auto, the owner proxy, gate authority by mode, profile setup, and the headless launch line.

## Context
≤35 new lines. In the Pipeline block add `auto    mode + vision → owner-proxy agent answers the grill, chains idea→plan→run, writes REPORT.md`. New section `## Auto mode (owner proxy)`: one paragraph on what it is and why DIVERGE tags exist (to measure whether a persona beats "yes to everything"); `cp templates/owner.md ~/.forge/owner.md`; a gate table with rows prototype / mvp / production and columns "proxy answers grill", "proxy approves", "human touch" (prototype: yes / all / pick mode, read report; mvp: yes / spec + tickets / final diff review in REPORT.md; production: yes / none, drafts only / spec, plan, final); launch line in a code block: `claude -p --permission-mode auto "/forge:auto prototype '<vision>'"`; Codex `$forge:auto`. Update the Layout block to `skills/{idea,plan,run,status,auto}/SKILL.md`, add `agents/owner.md`, and `templates/{PRODUCT,ticket,STATE,owner}.md`. Change "four skills" wording to five where it appears.

Notes from ticket 04: a headless production run ends at each human gate (spec, plan, final); re-running `/forge:auto` shows the gate again and approval needs an interactive reply (terminal or Remote Control) — say so in one sentence under the table. REPORT.md is rewritten whenever auto stops, and its last printed line is `<path>/REPORT.md — AGREE a, DIVERGE d, ESCALATE e`.

## Checks (run in order — cheapest first)
1. `claude plugin validate ~/dev/forge --strict`
2. `grep -q 'forge:auto' README.md && grep -q 'owner.md' README.md && grep -q 'permission-mode auto' README.md`

## Done when
- README carries the section, table, launch line and updated layout.

## Not included
- Anything outside README.md.
