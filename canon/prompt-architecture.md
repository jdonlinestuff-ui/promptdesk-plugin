---
last_verified: 2026-08-09
expires: 2026-11-07
governed_by: [claude-prompting-best-practices]
---

# Prompt architecture

The component order every prompt written by this plugin follows. Omit a component
when the task genuinely has no use for it — do not pad. Keep the order, because it
places longform data before the query, which is what the long-context guidance
calls for.

| # | Component | Purpose | Tag |
|---|---|---|---|
| 1 | Role | Who the model is for this task | `<role>` |
| 2 | Tone | Register and verbosity, when it matters | `<tone>` |
| 3 | Background | Standing context, domain facts, constraints | `<context>` |
| 4 | Task and rules | What to do, as numbered steps; what must hold | `<instructions>` |
| 5 | Examples | 3–5 relevant, diverse, structured | `<examples>` |
| 6 | Prior state | Conversation history or session state, if any | `<history>` |
| 7 | Input | The actual material to work on | `<input>` |
| 8 | Reasoning cue | Permission and direction for thinking | inline |
| 9 | Output format | Exact shape of the response | `<output_format>` |
| 10 | Rules | Hard limits, uncertainty permission, refusals | `<rules>` |

## Notes on individual components

**Role (1)** — one or two sentences. Names the job, not a personality. Goes in the
system prompt when the caller controls one.

**Background (3)** — include the *reason* the task matters, not only the facts.
Stating who the output is for and what it enables measurably improves results.

**Task and rules (4)** — numbered steps when order or completeness matters. State
scope explicitly; current models follow instructions literally and will not
silently generalise.

**Input (7)** — for anything over roughly 20k tokens, this block moves to the very
top, above components 1–6, and the query stays at the bottom.

**Reasoning cue (8)** — a general instruction ("think thoroughly before answering")
usually beats a prescriptive step list. Never phrase this as a request to display
or reproduce reasoning; see the model-specific traps in `guardrails.md`.

**Output format (9)** — say what to produce, not what to avoid. Use an XML tag or a
literal template. For guaranteed JSON, use Structured Outputs rather than prompt
wording.

**Rules (10)** — always include explicit permission to say "I don't know" and, when
sources exist, a requirement to ground claims in quoted source text.

## Skeleton

```
<role>…</role>

<context>
Why this task exists, who the output is for, what it enables.
</context>

<instructions>
1. …
2. …
</instructions>

<examples>
<example>…</example>
<example>…</example>
<example>…</example>
</examples>

<input>
"""
…
"""
</input>

<output_format>
…
</output_format>

<rules>
- Say "I don't know" rather than guessing.
- Ground every factual claim in a quote from the input.
- …
</rules>
```
