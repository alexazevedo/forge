---
id: 01
status: done
model: opus
depends_on: []
max_attempts: 2
files: [agents/owner.md, templates/owner.md, templates/PRODUCT.md, docs/discovery-auto-owner.md]
---
## Goal
Add the owner-proxy agent definition, the owner profile template, and the `owner:` frontmatter plus grill-log section to the PRODUCT.md template.

## Context
`docs/discovery-auto-owner.md` is read-only design context. Mirror `agents/reviewer.md` for shape.

`agents/owner.md` frontmatter: `name: owner`; `description: forge owner proxy — answers one grill question on the human's behalf when PRODUCT.md says owner: agent. Dispatched by forge:idea (and once by forge:plan for approval); never chooses the mode.`; `tools: Read, Grep, Glob, Skill`; `model: opus`. Body ≤40 lines: inputs are the vision, `~/.forge/owner.md` if it exists, PRODUCT.md so far, grill log so far, the question with forge's recommended answer, and the mode (given, never changed). First action: invoke the `product-manager` skill via the Skill tool if available, else a built-in stance (narrow ICP, one job, cut scope, observable done). Output exactly: `ANSWER:` (≤3 lines), `TAG: AGREE|DIVERGE|ESCALATE`, `REASON:` one line (required for DIVERGE/ESCALATE). Rules: never ask back, never pick the mode, smallest scope on ties, ESCALATE only when vision plus profile truly cannot decide, no implementation detail. For an approval request, ANSWER is `APPROVE` or a ≤5-line list of blocking objections.

`templates/owner.md`: header comment "copy to ~/.forge/owner.md"; sections Who, Defaults (stack, hosting, UI language, DB), Taste (3–5 bullets), Non-negotiables, Escalate when.

`templates/PRODUCT.md`: add `owner: human                  # human | agent (set by forge:auto)` after `parallel_default`; append optional `## Grill log` with table header `| # | question | recommendation | answer | tag |` and comment "present only when owner: agent".

## Checks (run in order — cheapest first)
1. `claude plugin validate ~/dev/forge --strict`
2. `grep -q '^tools:.*Skill' agents/owner.md && grep -q '^model: opus' agents/owner.md`
3. `grep -q '^owner: human' templates/PRODUCT.md && grep -q '^## Grill log' templates/PRODUCT.md && test -f templates/owner.md`

## Done when
- The three files exist with the fields above and validation passes.

## Not included
- Any change to skills or README.
