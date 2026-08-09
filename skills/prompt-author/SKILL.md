---
name: prompt-author
description: >
  This skill should be used when the user asks to "write a prompt", "rewrite this
  prompt", "improve this prompt", "review my prompt", "why isn't this prompt
  working", "make this prompt better", or "lint this prompt", or wants a reusable
  prompt built to documented best practice. Also use when a prompt is being
  written for a specific Claude model and needs checking against that model's
  known traps. For choosing which model a task should run on, and generating the
  system prompt that comes with that assignment, use prompt-router instead.
metadata:
  version: "0.2.0"
---

# Prompt author

Write new prompts and review existing ones against documented Anthropic guidance.

## Before writing or reviewing

Read these, in this order:

1. `${CLAUDE_PLUGIN_ROOT}/canon/prompt-architecture.md` — the component order
2. `${CLAUDE_PLUGIN_ROOT}/canon/best-practices.md` — the rules
3. `${CLAUDE_PLUGIN_ROOT}/canon/guardrails.md` — hallucination, consistency, and
   the per-model traps. This is the sole home of the traps; model files do not
   repeat them.

Read a model's detail file — `${CLAUDE_PLUGIN_ROOT}/canon/models/<model-id>.md` —
only for the one target model, and only when its behaviour matters to the prompt.
Start from `${CLAUDE_PLUGIN_ROOT}/canon/models/INDEX.md` if the ID is not yet known.
Never load the whole model set to check one prompt.

Each file carries an `expires` date. If today is on or after it, say so before
relying on the content, and point at `${CLAUDE_PLUGIN_ROOT}/canon/SOURCES.md`.

If no target model is known, ask which model the prompt runs on — or, when the
user does not know, hand off to `prompt-router` first. Model-specific traps cannot
be checked without a target.

## Writing a prompt

Follow the ten-component architecture. Omit a component only when the task has no
use for it; never pad to fill the list.

Apply these while drafting:

- Explicit over implicit. Assume no shared context with the reader.
- Give the reason behind each instruction, not only the instruction.
- Numbered steps when order or completeness matters.
- XML tags separating instructions, context, input and examples.
- Three to five examples when output shape matters — relevant, diverse, wrapped in
  `<example>` tags.
- Longform input above the query when the input exceeds roughly 20k tokens.
- State what to do, not what to avoid.
- Explicit permission to say "I don't know".
- Grounding requirement — quote the source before making a claim about it — when
  sources exist.

## Reviewing a prompt

Run the checklist below. Report findings as a numbered table with columns: item,
severity (blocker / warning / note), what is wrong, and the specific fix. Do not
rewrite the prompt unless the user asks; propose the change and let them decide.

### Structure

1. Role statement present and specific to the task
2. Purpose or reason given, not only the instruction
3. Instructions ordered; numbered where order matters
4. XML tags separating instructions, context, input, examples
5. Input placeholder present and unambiguous
6. Output format stated explicitly
7. Examples present when output shape matters — count between three and five
8. Examples diverse enough that no unintended pattern is learnable
9. Longform input placed above the query
10. Scope stated explicitly rather than left to inference

### Behaviour

11. Instructions phrased as what to do, not what to avoid
12. Permission to say "I don't know" present
13. Grounding or citation requirement present when sources exist
14. Tool use described conditionally ("use X when…") rather than as a blanket default
15. No anti-laziness or thoroughness padding inherited from older models
16. Destructive or irreversible actions gated behind confirmation, when the prompt
    drives an agent

### Model-specific traps

Check only those matching the target model. Each is a **blocker**.

17. **Opus 5** — no "double-check", "re-verify", "final verification step", or
    "use a subagent to verify". It self-verifies; these add cost without gain.
18. **Opus 5** — no rules telling it not to think or not to reason.
19. **Fable 5** — no "show your thinking", "reproduce your reasoning", or any
    request to display its thought process in the response. Triggers a
    `reasoning_extraction` refusal.
20. **Sonnet 5** — no `temperature`, `top_p` or `top_k` in the accompanying config.
    Returns a 400 error.
21. **Any 4.6+ target** — no prefilled assistant turn. Returns a 400 error.
22. **Any 4.7+ target** — no `thinking: {type: "enabled", budget_tokens: N}`.
    Returns a 400 error.
23. **Opus 4.8** — thinking is off by default; if the prompt assumes reasoning, the
    config must enable adaptive thinking.
24. **Fable 5 / Mythos 5** — if the caller requires zero data retention, the target
    is invalid; both are Covered Models on 30-day retention.
25. **Any model** — every model ID mentioned appears in `canon/models/INDEX.md`.

### Freshness

26. If the prompt hardcodes a price, model ID or capability claim, it carries a
    date or a pointer to where that fact is maintained.

## Output

For a **new prompt**: the prompt in a fenced block, then a short note listing which
architecture components were included and why any were left out, then the
model-specific configuration it needs.

For a **review**: the numbered findings table, then a one-paragraph verdict stating
whether the prompt is safe to run as-is. Offer the rewrite; do not perform it
unsolicited.

## Rules

- Judge against the canon, not against taste. If a criticism cannot be traced to a
  canon file, mark it as an opinion.
- Report warnings you cannot mechanically confirm rather than guessing.
- Never invent a model fact. Say "I don't know" and point at `SOURCES.md`.
- Do not add verification instructions to an Opus 5 prompt to be safe. That is the
  documented failure mode, not a safe default.
