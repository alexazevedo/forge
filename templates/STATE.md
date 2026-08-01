# STATE — rewrite after every ticket completes; never append. ≤30 lines total.

## Now
<ticket id + one line, or "idle">

## Next
<ready tickets in dependency order>

## Decisions
- execution: <sequential | parallel×N>   <!-- set once by forge:run, reused on resume -->
- <durable choices made mid-run, one line each>

## Blockers
- <ticket id — one-line reason>, or "none"
