---
last_verified: 2026-08-09
expires: 2026-11-07
---

## Claude Opus 5 — `claude-opus-5`

**Price:** $5/M input, $25/M output
**Specs:** 1M context (default and max), 128k max output

Deep reasoning, agentic and long-horizon work, test-time compute scaling.

**Strengths:** strongest on difficult coding — multi-file features, larger
refactors, end-to-end feature work — and completes tasks rather than leaving
stubs. High precision and recall in code review, holding accuracy at lower effort.
Strong chart, document and diagram understanding; excellent UI and frontend visual
replication. Coordinates subagent teams well with few overwrite collisions.

**Effort:** `high` default. `low` and `medium` give strong quality at a fraction of
tokens and latency. `xhigh` for demanding coding and agentic work. Effort defaults
carried over from earlier models may no longer apply — re-validate.

**Prompting:**
- Give the complete task specification up front and let it run.
- Default responses run long. Prompt explicitly for concision; lowering effort
  reduces thinking volume, not visible response length.
- It over-narrates during agentic work — define the update cadence explicitly.
- Constrain scope explicitly and constrain subagent spawning explicitly.
- Keep thinking enabled and control cost with effort. With thinking disabled it can
  write tool calls as text or leak internal XML tags into the response.

**Hard traps.** Prompt wordings and API settings that fail on this model are
listed once, in `canon/guardrails.md` under "Model-specific traps". Check there
before writing a prompt for it; they are deliberately not repeated here.
