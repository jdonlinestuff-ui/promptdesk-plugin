---
last_verified: 2026-08-09
governed_by: [reduce-hallucinations, increase-consistency, per-model prompting guides]
---

# Guardrails

## Reducing hallucination

| Technique | How to apply it |
|---|---|
| Allow uncertainty | Give explicit permission to say "I don't know". Drastically reduces false information for one line of prompt. |
| Quote before answering | For documents over ~20k tokens, ask for word-for-word extracted quotes first, then the task. Grounds the response in the actual text. |
| Cite per claim | Require a supporting quote for each claim. If no quote can be found, the claim must be retracted. |
| Chain-of-thought check | Ask for step-by-step reasoning before the final answer to expose faulty logic. |
| Best-of-N | Run the same prompt several times and compare. Divergence between runs signals hallucination. |
| Iterative refinement | Feed the output back and ask it to verify or expand. Catches inconsistencies. |
| Restrict to provided context | State that only the supplied documents may be used, not general knowledge. |

None of these eliminate hallucination. Validate anything high-stakes independently.

## Increasing consistency

- Define the output format precisely — JSON, XML, or a literal template.
- Constrain with examples rather than abstract description; examples work better.
- Ground repeated tasks in a fixed retrieved context so answers do not drift.
- Chain complex work into smaller subtasks, each getting full attention.
- Hold character through the system prompt — role, background, traits — plus
  worked scenarios with expected responses.
- Prefilling the assistant turn is unavailable on 4.6+. Use Structured Outputs for
  guaranteed JSON schema conformance rather than prompt engineering alone.

## Model-specific traps

Never write these into a prompt aimed at the named model. The linter in
`prompt-author` checks for each.

| Model | Never write | Why |
|---|---|---|
| Opus 5 | "double-check your answer", "re-verify before responding", "include a final verification step", "use a subagent to verify" | It self-verifies. These compound and add cost without improving results. |
| Opus 5 | Rules telling it not to think or not to reason | Increases internal XML tag leakage when thinking is disabled. |
| Fable 5 | "show your thinking", "reproduce your reasoning", "explain your thought process in the response" | Triggers a `reasoning_extraction` refusal. |
| Sonnet 5 | `temperature`, `top_p`, `top_k` in the API config | Returns a 400 error. Steer tone via the system prompt. |
| Any 4.6+ | Prefilled assistant turn | Returns a 400 error. |
| Any 4.7+ | `thinking: {type: "enabled", budget_tokens: N}` | Returns a 400 error. Use adaptive thinking plus `effort`. |
| Opus 4.8 | Assuming thinking is on | Thinking is off by default; enable adaptive thinking explicitly. |
| Fable 5 / Mythos 5 | Assuming zero data retention | Both are Covered Models on 30-day retention. |

## Anti-patterns in prompt wording

- Telling the model what to avoid instead of what to do.
- Blanket tool defaults ("always use X") where a conditional would serve
  ("use X when it would improve your understanding"). Current models over-trigger
  on blanket defaults.
- Anti-laziness prompting inherited from older models — current models are more
  proactive and will over-execute.
- Naming internal tags explicitly when trying to suppress them; general
  instructions work better.
