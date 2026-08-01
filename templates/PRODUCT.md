---
mode: prototype                # prototype | mvp | production
routing_profile: cloud         # cloud | cloud+local (local is opt-in, never a default)
parallel_default: sequential   # sequential | parallel
# local_exec: ollama launch claude --model <local-model> --yes -- -p "{ticket}"
#   ^ required iff routing_profile is cloud+local — command template the nested
#     local harness runs inside the ticket worktree. Alternatives:
#     pi -p --provider ollama ..., or raw `ollama run` for generation-only tickets.
---

# <name>

## What / why
<2–4 sentences. The problem, and why now.>

## Primary user story
As <who>, I <do what> so that <payoff>.

## Out of scope
- <explicitly not building>

## Success demo
Done when I can <one sentence, observable>.

<!-- body word caps by mode: prototype 250, mvp 600, production 1 page (+ ADRs if warranted) -->
