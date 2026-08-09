---
last_verified: 2026-08-09
governed_by: [claude-prompting-best-practices]
---

# Prompting best practices

The rules every prompt this plugin writes or reviews is held to.

## Clarity

- Be explicit. Treat the model as a brilliant new employee who lacks context on
  your norms and workflows.
- Golden test: show the prompt to a colleague with minimal context and ask them to
  follow it. If they would be confused, the model will be too.
- Give instructions as sequential numbered steps when order or completeness matters.
- Give the reason behind an instruction, not only the instruction. Models generalise
  from explanations.

## Structure

- Use XML tags to separate instructions, context, input and examples. Consistent,
  descriptive tag names; nest when content has natural hierarchy.
- Set a role in the system prompt to focus behaviour and tone.
- For long context (20k+ tokens), put longform data **above** the query,
  instructions and examples. Queries at the end can improve response quality by up
  to 30%.
- Wrap documents in `<document>` tags with `<document_content>` and `<source>` subtags.

## Examples

- Examples are the most reliable way to steer output format, tone and structure.
- Include three to five. Make them relevant (mirroring real use), diverse (covering
  edge cases, varied enough that no unintended pattern is learned), and structured
  (wrapped in `<example>` tags, multiple inside `<examples>`).

## Output control

- State what to do, not what to avoid. "Write in flowing prose paragraphs" beats
  "do not use markdown".
- Use XML format indicators when the shape matters.
- Match prompt style to desired output — removing markdown from a prompt reduces
  markdown in the response.
- Prefilling the assistant turn is **not supported** on Claude 4.6+ and Mythos
  Preview (returns 400). Use XML tags, explicit format instructions, system-prompt
  role setting, and Structured Outputs for guaranteed JSON.

## Thinking

- Current models use adaptive thinking; the model decides when and how much to
  think, calibrated by `effort` and query complexity.
- Manual extended thinking with `budget_tokens` is not supported on 4.7+ (400 error).
- Prefer general instructions over prescriptive steps — "think thoroughly" often
  beats a hand-written plan.
- To reduce over-triggering: "Thinking adds latency and should only be used when it
  will meaningfully improve answer quality — typically for problems that require
  multistep reasoning. When in doubt, respond directly."

## Tool use

- Be explicit about using tools. Without direction, models sometimes suggest changes
  rather than implementing them. "Make the following changes to [file]" beats "can
  you suggest some changes?"
- State the parallelism you want: independent calls in parallel, dependent calls
  sequential, never placeholder or guessed parameters.

## Agentic work

- Provide clear success criteria before a long run.
- Use structured formats (JSON) for state and test data; freeform text for progress
  notes; git for checkpoint history.
- Ask for tests in a structured file up front, and state that removing or editing
  them is unacceptable.
- Guard destructive actions: encourage local reversible actions, require
  confirmation for deletes, force-pushes, and anything visible to others.
- Constrain over-engineering: no features, refactors, docstrings, defensive
  handling or abstractions beyond what was asked.
- Require investigation before claims: never speculate about code that has not
  been opened.

## Common failure to design out

Models converge on generic output. For frontend work especially, name concrete
typography, palette hexes, motion and background treatment rather than saying
"make it clean" — a generic instruction just selects a different default.
