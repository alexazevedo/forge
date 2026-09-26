# STATE — forge:auto owner-proxy (prototype)

## Now
- idle — all 6 tickets done, smoke green (2026-09-26)

## Next
- none. Prototype finish: all checks green = done, approval budget spent at plan.
- Not committed yet. To use /forge:auto: bump version, push, `claude plugin marketplace update forge-marketplace && claude plugin update forge`, restart — or `claude --plugin-dir ~/dev/forge`.

## Decisions
- Execution: sequential in place.
- Owner output contract: ANSWER / TAG AGREE|DIVERGE|ESCALATE / REASON; approval = APPROVE tagged AGREE, objections tagged DIVERGE.
- Grill log table in PRODUCT.md is outside the body word cap.
- YAML descriptions containing ": " must be double-quoted; `claude plugin validate` only checks marketplace.json.
- Headless production runs end at each human gate; approval needs an interactive reply.
- Smoke mechanics: no `timeout` on macOS, Bash call cap 10 min → nohup + perl alarm + poll.

## Blockers
- none
