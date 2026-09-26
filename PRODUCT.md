---
mode: prototype
routing_profile: cloud
parallel_default: sequential
---

# forge:auto — owner-proxy pipeline

## What / why
Alex has ideas he lacks the patience to grill through, even with forge.
`/forge:auto [mode] "<vision>"` runs idea → plan → run with a persona-backed
**owner agent** answering every grill question and holding gate authority
according to the mode. The human decides only the product class. The grill
transcript stays auditable, so the final report shows where the proxy
diverged from forge's own recommendation — the signal that tells us whether
a persona beats "yes to everything".

## Primary user story
As Alex, I run `/forge:auto prototype "build a clone of X"` — from a terminal,
headless `claude -p`, Remote Control, or Codex — so that PRODUCT.md, tickets,
and working code appear with zero further prompts, plus a report of
AGREE / DIVERGE / ESCALATE answers.

## Out of scope
- Night script / `caffeinate` wrapper; Telegram, voice, WhatsApp ingress
- Deploy to the VPS; multi-project board; decomposing a vision into cycles
- Codex as the proxy; hiding the recommendation from the proxy
- Personas inlined into forge skills; native app

## Success demo
Done when I can run `/forge:auto prototype "<vision>"` in a scratch repo and
get PRODUCT.md with `owner: agent` and a tagged grill log, tickets, a
completed run with zero human prompts, and a divergence count in the final
report — with `claude plugin validate ~/dev/forge --strict` still passing.

<!-- design decisions already taken: docs/discovery-auto-owner.md -->
