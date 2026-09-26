---
name: auto
description: "Use when the user wants forge to run end-to-end with an owner-proxy agent answering the grill — they decide only the product class. Triggers on /forge:auto, $forge:auto, 'auto mode', 'run it without asking me'."
---

# forge:auto — idea → plan → run, owner answers

## Resume (check first)

PRODUCT.md says `owner: agent` → resume: ask nothing, keep its mode, and
re-enter at the first unfinished stage — with STATE.md and tickets present
that is forge:status, then "continue" → forge:run. An open ESCALATE or a
pending production gate: show it again, wait for the human.

## Mode — the only question

`/forge:auto [mode] "<vision>"`, mode = prototype | mvp | production.
Mode missing → ask exactly ONE question with a recommended mode (so "yes"
works); if the vision is missing too, ask for both in that same message
(vision alone missing: ask only that). Nothing else is ever asked. Then
say once: no further prompts except production gates; headless runs need
`claude -p --permission-mode auto "/forge:auto <mode> '<vision>'"`.

## Chain

Invoke each stage via the Skill tool (Codex: `$forge:idea` …), follow its
`owner: agent` rules, never stop between stages ("next: …" = invoke now).

1. forge:idea with the vision, `mode: <mode>`, `owner: agent` (dial
   answered; forge:owner takes the grill). PRODUCT.md frontmatter gets
   `mode: <mode>` and `owner: agent` the moment it is written.
2. forge:plan — owner approval (production: human).
3. forge:run — execution from `parallel_default`, never asked. Then REPORT.md.

| mode | gate authority |
|---|---|
| prototype | owner approves all; no final gate |
| mvp | owner approves spec + tickets; diff summary → REPORT.md |
| production | owner drafts; human approves spec, plan, final |

## Hard stops

- Production gates wait for the human's explicit approval — never the
  owner's, never yours — then continue. Headless, the run ends there.
- ESCALATE in mvp / production stops until the human answers it.
- Never lower the mode — not to pass a gate, a stop or a failing check.

## REPORT.md (repo root; rewrite whenever auto stops)

- vision, mode, outcome: done | waiting on <gate> | stopped: <why>
- `AGREE a, DIVERGE d, ESCALATE e` counted from `## Grill log` (zeros
  too); every DIVERGE and ESCALATE row in full; the owner's plan verdict
- ticket board as forge:status prints it; blocked tickets with diagnosis
- how to run the result: the last ticket's runtime-probe command
- mvp: the diff summary — keep forge:run's `Final review` section

Last printed line: `<abs path>/REPORT.md — AGREE a, DIVERGE d, ESCALATE e`.
