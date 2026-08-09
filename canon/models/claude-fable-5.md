---
last_verified: 2026-08-09
expires: 2026-11-07
---

## Claude Fable 5 — `claude-fable-5`

**Price:** $10/M input, $50/M output
**Specs:** 1M context (default and max), 128k max output, thinking always enabled

Anthropic's most capable widely released model, built for the most demanding
reasoning and long-horizon agentic work.

**Strengths:** long-horizon autonomy across multiday goal-directed runs with
strong instruction retention; first-shot correctness on complex systems; dense
technical images and screenshots; enterprise workflows including financial
analysis, spreadsheets, slides and documents; high-recall code review and
repository-wide search; navigating ambiguous multithreaded requests; managing
parallel subagents.

**Effort:** `high` default. `xhigh` for the most capability-sensitive work.
`medium` or `low` for routine work — still outperforms `xhigh` on prior models.

**Constraints:**
- Thinking cannot be disabled. Reduce depth with `effort`.
- Raw thinking is never returned. Set `thinking.display` to `"summarized"` for a
  readable summary; default is `"omitted"`.
- Covered Model on 30-day retention — **not available under zero data retention**.
- Safety classifiers can decline a request, returning `stop_reason: "refusal"` as
  an HTTP 200. Callers must handle that path. Refused requests are not charged,
  and fallback credit refunds prompt-cache switching costs.
- Declines offensive cybersecurity work and sensitive life-sciences content.
- Availability: Claude API, Amazon Bedrock, Google Cloud Vertex AI, Microsoft Foundry.

**Prompting:**
- Start at your difficulty ceiling. Assign harder tasks than to prior models.
- Turns run long — raise client timeouts and enable streaming before migrating.
- Tell it to act when it has enough information rather than re-deriving settled
  facts or narrating options it will not pursue.
- Require it to audit every progress claim against an actual tool result from the
  session, and to say so explicitly when something is unverified.
- Tell it to report findings and stop when the user is describing a problem rather
  than requesting a change.
- Constrain refactoring: no abstractions, error handling or cleanup beyond the task.
- Put verification in a separate verifier subagent, not self-critique.
- For unattended runs, state that the user is not watching and cannot answer
  mid-task, and that any closing plan or promise must be executed before the turn ends.

**Hard traps.** Prompt wordings and API settings that fail on this model are
listed once, in `canon/guardrails.md` under "Model-specific traps". Check there
before writing a prompt for it; they are deliberately not repeated here.
