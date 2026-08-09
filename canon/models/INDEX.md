---
last_verified: 2026-08-09
expires: 2026-11-07
---

# Model selection index

Read this first. Then read **only** the detail files for the models you are
actually choosing between — `canon/models/<model-id>.md`. Do not load all six.

If today is on or after the `expires` date above, say the model facts may be
stale and point at `canon/SOURCES.md` before this table drives a cost,
availability or compliance decision.

| Model | ID | Best for | Price (in / out per M) |
|---|---|---|---|
| Claude Fable 5 | `claude-fable-5` | Highest capability; long-horizon autonomy, multiday agent runs, enterprise document and financial work, high-recall review | $10 / $50 |
| Claude Mythos 5 | `claude-mythos-5` | Fable 5 capabilities without safety-classifier refusals; Project Glasswing only | $10 / $50 |
| Claude Opus 5 | `claude-opus-5` | Deep reasoning, multi-file features, larger refactors, high-precision code review, subagent coordination | $5 / $25 |
| Claude Opus 4.8 | `claude-opus-4-8` | Long-horizon agentic and knowledge work, vision, memory, design and frontend generation, interactive coding | not listed |
| Claude Sonnet 5 | `claude-sonnet-5` | Frontier intelligence at speed; coding agents, agentic search, structured extraction, data analysis, code review | not listed |
| Claude Haiku 4.5 | `claude-haiku-4-5` | Real-time, high-volume, cost-sensitive work and sub-agent tasks | not listed |

## Effort ladder

`low` → `medium` → `high` → `xhigh` → `max`

Effort trades intelligence against latency and cost **within a single model**.
Before moving a unit of work up a model tier, check whether raising effort on the
current model closes the gap — it is usually cheaper.

Per-model effort defaults differ; they are in each model's detail file.

## Access and compliance filters

Apply these before assigning, not after:

- **Mythos 5** requires Project Glasswing. Otherwise assign Fable 5, the documented
  fallback with identical capabilities.
- **Fable 5 and Mythos 5** are Covered Models on 30-day retention — unavailable
  under zero data retention.
- **Fable 5** can return `stop_reason: "refusal"` (HTTP 200) on offensive-security
  and sensitive life-sciences work. Specify an Opus 4.8 fallback rather than
  assigning it bare. Refused requests are not charged.

## Hard traps

Never write a model-specific trap into a prompt. All of them live in one place:
`canon/guardrails.md`, section "Model-specific traps". Check there, not here.
