# Brief: `forge:auto` with an owner-proxy agent

Source: product/architecture discussion, 2026-09-25. Treat every item below as
an already-answered grill question.

## Dial
- mode: **prototype**
- greenfield: no — existing repo `~/dev/forge` (the plugin itself)
- routing_profile: cloud, parallel_default: sequential

## Who / job
Alex, solo builder. Invokes forge from a terminal, headless (`claude -p`),
Remote Control from the phone, or Codex. Has ideas he lacks the patience to
grill through, even with forge. Wants to decide **only the product class**
(prototype / mvp / production) and let the pipeline run to completion.

One job: a single command `/forge:auto [mode] "<vision>"` runs
idea → plan → run with a persona-backed **owner agent** answering every grill
question and holding gate authority according to the mode.

## Decisions already made
- New skill `skills/auto/SKILL.md` (~50 lines): captures mode (argument, or
  exactly one question with a recommendation when absent), writes
  `owner: agent` into PRODUCT.md frontmatter, chains idea → plan → run, prints
  a final report including the grill log (AGREE / DIVERGE / ESCALATE counts),
  resumes from STATE.md without re-asking.
- New agent `agents/owner.md` (~40 lines, model opus): stateless per question.
  Inputs: vision sentence, `~/.forge/owner.md` profile if present, PRODUCT.md
  so far, grill log so far, the question with forge's recommended answer.
  Persona: invoke the `product-manager` skill via the Skill tool if available,
  else a built-in product-owner stance. Output ≤3 lines, tagged
  `AGREE` / `DIVERGE` (+ one-line reason) / `ESCALATE`. Never asks back.
  Never chooses the mode.
- `templates/owner.md`: profile template (who the owner is, stack/hosting
  defaults, language, taste, non-negotiables). Copied by the user to
  `~/.forge/owner.md`.
- `templates/PRODUCT.md`: frontmatter gains `owner: human   # human | agent`;
  body gains optional `## Grill log` (Q, recommendation, answer, tag).
- `skills/idea`: if `owner: agent`, route each grill question to
  `agents/owner.md` instead of the user; append to the grill log. Mode is
  never routed to the owner.
- `skills/plan` gate under `owner: agent`: prototype and mvp → owner approves;
  production → human approves asynchronously (notify, wait).
- `skills/run` gate under `owner: agent`: prototype → no gate; mvp → diff
  summary goes into the report; production → reviewer pass, then human final
  review asynchronously. The run question (parallel vs sequential) is answered
  from `parallel_default`, never asked.
- ESCALATE handling: prototype → pick the smallest-scope option and continue;
  mvp / production → pause, record in STATE.md Blockers, notify.
- `skills/status`: board line shows owner mode and divergence count.
- Codex host: owner body inlined as role instructions, same as implementers.
- README: `auto` section, gate-authority table by mode, launch line
  `claude -p --permission-mode auto "/forge:auto prototype '<vision>'"`.
- Keep skills total ≤600 lines. `claude plugin validate --strict` must pass.

## Out of scope
- Night script / `caffeinate` wrapper (separate ticket after auto proves out)
- Telegram, voice, WhatsApp ingress; notifications beyond a terminal line
- Deploy to the VPS; multi-project board; decomposing a vision into cycles
- Codex as the proxy; hiding the recommendation from the proxy
- Personas embedded inline in forge skills; native app

## Success demo
Done when `/forge:auto prototype "<vision>"` in a scratch repo produces
PRODUCT.md with `owner: agent` and a tagged grill log, tickets, runs to
completion with zero human prompts, and prints a report with the divergence
count — and `claude plugin validate ~/dev/forge --strict` passes.
