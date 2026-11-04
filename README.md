# agent-rules

This repository versions workspace agent instructions. `AGENTS.md` defines the policy files as `AGENTS.md` and `RULES.md`; this README is not policy.

## Unadopted draft: regression-causing fixes

The governing text is the `RULES.md` Reviews bullet beginning “When fixing findings.” The wording below is a draft recorded against baseline `acc2243cdae350bcf89de4d75ae6e381af69816a`; its presence is not evidence of approval or adoption.

Compared with the governing rule, the draft changes these requirements:

- A mandatory replacement-first order becomes a preference that also permits revision.
- A correct fix that introduces no regression becomes a fix for which the required verification finds no regression.
- Reimplementing from the last reviewed version “if needed” becomes doing so “when useful.”
- Compensation only when no qualifying replacement is possible becomes compensation when causal removal is impractical or materially riskier.
- Fixing the regression on top becomes compensating for its effects.

These differences may weaken safeguards. Adopting the draft requires explicit user approval under `AGENTS.md`, an update to `RULES.md`, identification of every weakening against the then-current governing rule, and review of that exact candidate. If the governing rule changes, this draft is stale and must not be relied on.

> **Not adopted; not an instruction.**
>
> When a fix causes a regression, prefer removing the regression's cause by revising or replacing the fix—reimplementing it from the last reviewed version when useful—so that it still resolves the original finding and the required verification finds no regression, rather than retaining that cause and compensating for its effects. Compensate only when such removal, whether by revision or replacement, is impractical or materially riskier than compensating, and document why.
