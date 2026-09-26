---
name: run
description: Use after tickets are approved to dispatch implementer subagents with bounded implement→check→fix loops. Triggers on /forge:run, $forge:run, or "continue" from forge:status.
---

# forge:run — dispatch + loop

Read PRODUCT.md and tickets/. Ask exactly ONE question before dispatching:

> Parallel in git worktrees (N concurrent, independent tickets only) or
> sequential in place? [include recommendation]

Default sequential (seed from `parallel_default`). Recommend parallel only
when ≥2 ready tickets have disjoint `files:`. Cap concurrency at 3 (4 max) —
beyond that, merge conflicts and human review capacity are the bottleneck,
not compute. Record the choice under STATE.md → Decisions; on resume, reuse
it and do NOT re-ask.

Under `owner: agent`: skip the question — answer from `parallel_default`,
recorded under STATE.md → Decisions the same way.

## Dispatch (per ticket)

Fresh subagent, never inherits session history. Prompt = the ticket file +
PRODUCT.md + the ticket's listed file paths. Detect the host from its available
agent primitives; do not ask the user which host is running.

- Claude Code: select the custom agent definition from the ticket's `model:`
  field → agents/implementer-{haiku,sonnet,opus}.md.
- Codex: spawn a fresh worker subagent at the corresponding routing tier from
  forge:plan. Include the BODY (everything after frontmatter) of the matching
  agents/implementer-{haiku,sonnet,opus}.md in the prompt as role
  instructions. If exact model selection is unavailable, use the inherited
  model and preserve the selected role instructions; the Claude model labels
  are routing tiers, not a reason to block dispatch.

Set `status: doing` in the ticket before dispatch.

## The loop (inside the implementer, bounded)

1. Implement.
2. Run checks in order; on first failure → fix → rerun. One full failed
   cycle = one attempt.
3. All checks green → done. `max_attempts` exhausted → stop, report honestly.

## Escalation (orchestrator, once per ticket)

At `max_attempts`: dispatch ONCE, one model rung up (haiku→sonnet→opus;
local→haiku), fresh context, prompt = ticket + condensed failure summary
(≤10 lines). Still failing → set `status: blocked`, append a 5-line
diagnosis to the ticket, move on to the next ready ticket. Never
infinite-loop; never silently lower the bar.

## local tickets (only when routing_profile: cloud+local)

`model: local` runs via a NESTED HARNESS, not a native host subagent:
Bash-exec the `local_exec:` command template from PRODUCT.md inside the
ticket's worktree. Sequential → the orchestrator execs it directly; parallel
→ a thin lowest-cost cloud worker execs it. Harness stdout goes to a log file,
never into orchestrator context. The local model NEVER judges its own work —
check execution, attempt counting, and escalation stay with the orchestrator.
Confine the harness to the worktree.

## After every ticket

- Update ticket `status:` (done | blocked).
- REWRITE STATE.md from templates/STATE.md (≤30 lines): Now / Next /
  Decisions / Blockers. Rewrite, never append.
- Read the implementer report (≤200 words). If it flags something affecting
  other tickets, update those tickets now, before dispatching them.

## Integration

Sequential, in dependency order. A worktree merge conflict becomes its own
micro-ticket (usually haiku). Never batch-merge.

## Finish

- prototype: all checks green = done. No further gate — approval budget
  (1) already spent at plan.
- mvp: show a diff summary for final review. (Approval 2 of 2.)
- production: one reviewer pass over the WHOLE diff, single round. Claude Code
  uses agents/reviewer.md as its custom agent; Codex spawns a fresh reviewer
  subagent with that file's body as role instructions. Findings become new
  tickets; no re-review ping-pong. Then final review with the user. (Approval
  3 of 3.)
- Under `owner: agent`: prototype → same, no gate. mvp → write the diff
  summary into `REPORT.md` under "Final review" instead of waiting.
  production → same reviewer pass, then stop and say final review is
  pending (human approval stays).
