---
name: prompt-router
description: >
  This skill should be used when the user asks "which model should I use",
  "which Claude model is best for this", "route this task", "how should I split
  this work", "what effort level", "is Opus overkill for this", "should this be
  Haiku or Sonnet", or otherwise needs a task assigned to a Claude model, an
  effort setting, and a role. Also use when the user is designing a multi-agent
  or multi-step pipeline and needs to know which model plays which part.
metadata:
  version: "0.2.0"
---

# Prompt router

Assign work to Claude models. Produce a model, an effort setting, a role, and a
ready-to-paste system prompt for each unit of work.

## Before routing

Read `${CLAUDE_PLUGIN_ROOT}/canon/models/INDEX.md` — the selection table, effort
ladder and access filters. That is usually enough to shortlist.

Then read **only** the detail files for models actually in contention:
`${CLAUDE_PLUGIN_ROOT}/canon/models/<model-id>.md`. Loading all six on a routine
route is waste; two or three is normal.

These files are the only source for model IDs, prices, effort levels,
capabilities and constraints. Never state a model fact that is not in them.

Each carries an `expires` date. If today is on or after it, tell the user the
model facts may be stale and point at `${CLAUDE_PLUGIN_ROOT}/canon/SOURCES.md`
before they act on pricing, availability or compliance details.

When the task maps to a recognised working role, read
`${CLAUDE_PLUGIN_ROOT}/canon/personas/INDEX.md` and then the one persona file
that fits, so manual persona use and automatic routing agree.

Read `${CLAUDE_PLUGIN_ROOT}/canon/guardrails.md` before writing any system
prompt — it holds the model-specific traps, which the model detail files
deliberately do not repeat.

## Procedure

1. **Restate the task** in one sentence. If the restatement would surprise the
   user, ask rather than route.

2. **Decompose.** List the distinct units of work. A unit is distinct when it has
   a different capability demand, latency need, or volume from its neighbours.

3. **Score each unit** on four axes:
   - capability demand — low / moderate / high / frontier
   - latency sensitivity — none / moderate / hard real-time
   - volume and cost exposure — one-off / recurring / high-volume
   - cost of a wrong answer — low / moderate / severe

4. **Choose a posture** for the task overall:
   - *Efficiency-first* for prototyping, tight latency, cost-sensitive work and
     high-volume straightforward tasks. Start at Claude Haiku 4.5, test, upgrade
     only on measured shortfall.
   - *Capability-first* for complex reasoning, scientific and mathematical work,
     nuanced understanding and accuracy-critical output. Start at Claude Opus 5,
     tune prompts there, then lower effort or step down once quality is stable.

5. **Assign a model per unit.** Before moving up a tier, check whether raising
   `effort` on the current model closes the gap. Effort trades intelligence
   against latency and cost inside one model and is cheaper than a tier change.

6. **Assign a role per unit** — Orchestrator, Architect, Implementer, Reviewer,
   Verifier, Extractor, Classifier, Synthesiser, Researcher, Designer, Drafter, or
   a better-fitting name. Where two units share a model but differ in role, keep
   them as separate calls with separate system prompts. One call wearing two hats
   produces worse output than two focused calls.

7. **Write each role's system prompt**, applying that model's prompting rules
   from its detail file, plus the architecture in
   `${CLAUDE_PLUGIN_ROOT}/canon/prompt-architecture.md` and the traps in
   `${CLAUDE_PLUGIN_ROOT}/canon/guardrails.md`.

8. **Check availability and compliance.** Rule out models the user cannot use:
   Mythos 5 needs Project Glasswing; Fable 5 and Mythos 5 are Covered Models on
   30-day retention and unavailable under zero data retention; Fable 5 can return
   `stop_reason: "refusal"` on offensive-security and sensitive life-sciences
   work, so specify an Opus 4.8 fallback rather than assigning it bare.

9. **Sanity-check cost.** If the plan spends frontier tokens on work a cheaper
   role could do, revise before answering.

## Escalation signals

Push **up** a tier when: multi-step autonomy with no human checkpoint; ambiguity
requiring the model to choose its own approach; large-scale refactoring, complex
systems engineering, vision-heavy workflows, computer use; long-horizon coherence
across hours or a very large context; a wrong answer is expensive, hard to detect,
or lands in front of a customer, regulator or executive.

Push **down** a tier when: the work is mechanical, templated, or has one correct
output shape; extraction, classification, formatting, routing or short-input
summarising; it runs many times per minute or as one leaf in a fan-out; a cheap
verification step already exists downstream.

## Output

Respond in this structure.

**Task read as:** one sentence.

**Posture:** efficiency-first or capability-first, with one clause of justification.

**Assignment** — a table with columns: unit of work, model and ID, role, effort.

**Why this split:** three to five sentences naming the specific properties of this
task that drove each choice. No generic model marketing.

**Per-role system prompts:** one fenced block per row. Each opens with a one-line
role statement, states the deliverable and its exact format, carries the
model-specific guardrails that apply, and gives permission to flag uncertainty
rather than guess. Keep each under 200 words.

**Cheaper alternative:** a specific downgrade and the condition under which it
holds, or "none — the step down loses [specific capability]".

**Escalate when:** a concrete observable failure per assignment, such as "it drops
constraints after step four" or "cross-file edits come back mutually inconsistent".

**Config notes:** only what applies — effort, thinking on or off, `max_tokens`
headroom, client timeouts, sampling parameters that error, fallback routing.

**How to verify:** two benchmark checks on the user's own prompts and data, naming
what to measure and on which cases. Benchmarking is the deciding step; the
assignment is a hypothesis until it runs.

## Rules

- One model per unit of work. Do not hedge between two.
- Never recommend a larger model without naming a concrete capability the smaller
  one lacks for this task. If none can be named, recommend the smaller one.
- Never invent a model, ID, price or effort level absent from the canon. If a fact
  is not in the index or the detail file you read, read the detail file rather
  than filling the gap from memory.
- If the user's available models differ from the canon, work with theirs and name
  the closest substitute for anything missing.
- Never assign a model the user cannot access. Access, retention posture and
  refusal handling are part of the recommendation, not footnotes.
- If the task is too vague to route, ask exactly one clarifying question — the one
  whose answer most changes the assignment — and stop.
- Say "I don't know" rather than guessing at a capability.
- Prefer fewer models. A three-model pipeline has to earn its handoffs.
