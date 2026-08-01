---
name: plan
description: Use after PRODUCT.md exists to slice the work into agent-ready tickets with executable checks and model routing. Triggers on /forge:plan.
---

# forge:plan — tickets

Read PRODUCT.md first; its `mode` sets every budget below. Planning always
runs on the strongest available model — plan quality gates everything
downstream. If the current session model is weaker, say so before planning.

## Slicing

- Vertical slices (tracer bullets): every ticket cuts through to observable
  behavior. Never layer-slice ("all models", then "all routes").
- Sized to fit one subagent context comfortably and require ZERO discovery:
  the implementer gets the ticket + PRODUCT.md + listed files, nothing else.
  If it would need to hunt, the ticket is underspecified — fix the ticket.
- One file per ticket: `tickets/NN-slug.md`, from templates/ticket.md.
- If parallel execution is plausible, make `files:` sets disjoint between
  independent tickets — parallel agents colliding on shared files is the #1
  documented multi-agent failure mode.

## Ticket fields

- `max_attempts`: from mode — prototype 2, mvp 3, production 4.
- Checks: executable commands whose exit codes define done, cheapest first
  (lint → build → test → runtime probe). Test checks appear mvp and up only.
  No prose criteria — "should work smoothly" is not a check.
- "Not included": explicit, so the agent doesn't gold-plate.

## Model routing (assign per ticket; user may override)

| model | use for |
|---|---|
| haiku | boilerplate, renames, docs, config, glue code, simple CRUD |
| sonnet | default implementation, refactors, tests |
| opus | architecture decisions, gnarly debugging, security-sensitive code |
| local | opt-in lane, haiku-class task types ONLY — see below |

Mixed routing cuts spend 40–60% versus running everything on one big model.

## local lane (strictly opt-in)

Tag `model: local` only when PRODUCT.md says `routing_profile: cloud+local`,
and only on haiku-class tickets. Never silently substitute local for cloud
or cloud for local. Execution mechanics live in forge:run — the plan only
tags.

## Output + gate

Print a one-screen summary: tickets in dependency order, model per ticket,
which are independent (parallelizable). Then:

- prototype / mvp: ONE gate — approve PRODUCT.md + tickets together.
  (prototype: approval 1 of 1. mvp: approval 1 of 2.)
- production: approve the plan. (Approval 2 of 3.)

Nothing runs before this approval.
