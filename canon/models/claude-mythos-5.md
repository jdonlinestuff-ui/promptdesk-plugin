---
last_verified: 2026-08-09
expires: 2026-11-07
---

## Claude Mythos 5 — `claude-mythos-5`

**Price:** $10/M input, $50/M output
**Specs:** identical capabilities to Fable 5 — 1M context, 128k output, always-on thinking

Differs from Fable 5 only in that it carries no safety classifiers, so it returns
no refusals. Limited release through Project Glasswing; access via an Anthropic,
AWS or Google Cloud account team. Also a Covered Model on 30-day retention.

**Assign only when** the caller states they are in Project Glasswing and the
workload genuinely hits Fable 5 refusals. Otherwise assign Fable 5, the documented
fallback with the same capabilities.

**Hard traps.** Prompt wordings and API settings that fail on this model are
listed once, in `canon/guardrails.md` under "Model-specific traps". Check there
before writing a prompt for it; they are deliberately not repeated here.
