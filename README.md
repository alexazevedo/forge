# forge

Right-sized, spec-driven delivery for Claude Code. A raw idea becomes shipped
code via grilling → agent-optimized tickets → model-routed subagent execution
with bounded verification loops. The core primitive is a **rigor dial**:
process weight scales with product class, never with tool habit.

forge replaces heavyweight single-gear pipelines (7–10 stages, 6–7 human
gates, identical ceremony for a throwaway prototype and a production system)
with four skills and a hard interaction budget per mode.

## Pipeline

```
/forge:idea    grill the raw idea → PRODUCT.md   (one question per message)
/forge:plan    vertical-slice tickets with executable checks + model routing
/forge:run     dispatch implementer subagents, bounded implement→check→fix loops
/forge:status  board + cold resume from PRODUCT.md + STATE.md only
```

## The dial

| | prototype | mvp | production |
|---|---|---|---|
| grill questions (max) | 4 | 8 | as needed |
| spec size (max) | 250 words | 600 words | 1 page + ADRs |
| human approvals (total) | 1 (spec+tickets) | 2 (+ final review) | 3 (spec, plan, final review) |
| tests | smoke only | core logic only | full discipline + reviewer pass |
| loop attempts / ticket | 2 | 3 | 4 |
| shortcuts | hardcode, TODOs, zero abstractions | some, documented | none |

Prototype mode explicitly PERMITS what heavyweight processes forbid: no unit
test suites, no speculative abstraction, no refactoring beyond the ticket.

## Install

```bash
git clone https://github.com/alexazevedo/forge ~/dev/forge
claude --plugin-dir ~/dev/forge        # try it in one session
# or add to your marketplace / plugin config for permanent install
claude plugin validate ~/dev/forge --strict
```

## Cost routing

The planner assigns a model per ticket; mixed routing cuts spend 40–60%
versus running everything on the strongest model.

| model | tickets | agent def |
|---|---|---|
| haiku | boilerplate, renames, docs, config, glue, simple CRUD | agents/implementer-haiku.md |
| sonnet | default implementation, refactors, tests | agents/implementer-sonnet.md |
| opus | architecture, gnarly debugging, security-sensitive | agents/implementer-opus.md |
| local | opt-in lane, haiku-class tasks only | nested harness (below) |

Planning itself always runs on the strong model — plan quality gates
everything downstream. Escalation ladder on repeated check failure:
one rung up (haiku→sonnet→opus; local→haiku), once, fresh context.

## Local model lane (opt-in)

Native Claude Code subagents cannot run non-Anthropic models, so `model:
local` tickets execute via a nested harness: the orchestrator (or a thin
haiku wrapper, for parallel local tickets) Bash-execs a headless external
agent inside the ticket's worktree.

1. Install a runner, e.g. [Ollama](https://ollama.com) with a coding model:
   `ollama pull qwen2.5-coder` (any haiku-class model).
2. Set the profile and command template in PRODUCT.md frontmatter:

```yaml
routing_profile: cloud+local
local_exec: ollama launch claude --model qwen2.5-coder --yes -- -p "{ticket}"
# alternatives: pi -p --provider ollama ...   |   raw `ollama run` for generation-only tickets
```

Rules the harness enforces: stdout goes to a log file, never into
orchestrator context; the local model never judges its own work (checks,
attempt counting, escalation stay with the orchestrator); failure at
`max_attempts` re-dispatches to cloud haiku with fresh context; the harness
is confined to the worktree. Scope local tickets to haiku-class work only.

## Layout

```
.claude-plugin/plugin.json
skills/{idea,plan,run,status}/SKILL.md   # the whole process, ≤600 lines total
agents/implementer-{haiku,sonnet,opus}.md
agents/reviewer.md                       # production mode only
templates/{PRODUCT,ticket,STATE}.md
```

## Philosophy

1. Rigor is a dial, not a default.
2. Simplest thing that works; complexity only when needed.
3. Verification is executable, not prose — exit codes define done.
4. Human attention is the scarcest resource — each mode has a hard
   interaction budget; exceeding it is a bug.
5. Context is a budget — small stable files, rewrite-not-append state,
   subagent reports ≤200 words.
