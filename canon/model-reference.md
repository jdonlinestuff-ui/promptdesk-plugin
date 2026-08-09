---
last_verified: 2026-08-09
governed_by: [choosing-a-model, introducing-fable-5-mythos-5, per-model prompting guides]
---

# Model reference

Authoritative table for every model fact used by this plugin. Never state a model
ID, price, effort level or capability that is absent here. If a caller needs
something this table does not cover, say so rather than filling the gap.

If today is more than 90 days after `last_verified` above, warn the user that
these facts may be stale and point them at `SOURCES.md` before relying on the
table for a cost or compliance decision.

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
- **Never** instruct it to "show your thinking" or reproduce its reasoning — this
  triggers a `reasoning_extraction` refusal.
- For unattended runs, state that the user is not watching and cannot answer
  mid-task, and that any closing plan or promise must be executed before the turn ends.

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
- **Do not** add "verify your work", "double-check" or "use a subagent to verify".
  It self-verifies, and these cause over-verification cost with no quality gain.
- Constrain scope explicitly and constrain subagent spawning explicitly.
- Keep thinking enabled and control cost with effort. With thinking disabled it can
  write tool calls as text or leak internal XML tags into the response.

---

## Claude Opus 4.8 — `claude-opus-4-8`

**Price:** not listed in sources

**Strengths:** long-horizon agentic work, knowledge work, vision, memory, code
review and bug finding, design and frontend generation. Good for interactive
synchronous coding sessions.

**Effort:** `max` for intelligence-demanding tasks (watch for overthinking and
diminishing returns). `xhigh` for most coding and agentic work. `high` as the
minimum for intelligence-sensitive work. `medium` for cost-sensitive. `low` only
for short, scoped, latency-sensitive tasks.

**Prompting:**
- Thinking is **off by default** — enable `thinking: {type: "adaptive"}` if needed.
- It favours reasoning over tool calls. Raise effort and state explicitly when and
  why to use each tool.
- It follows instructions literally and will not silently generalise. State scope:
  "apply this to every section, not just the first one."
- At `max` or `xhigh`, set a large max output token budget — start at 64k.
- Default design house style is warm cream (~`#F4F1EA`), serif display type,
  italic accents, terracotta/amber. Override with concrete hexes and typefaces for
  dashboards, dev tools, fintech, healthcare and enterprise work.
- Use as the fallback target when Fable 5 refuses.

---

## Claude Sonnet 5 — `claude-sonnet-5`

**Price:** not listed in sources

Frontier intelligence at scale, fast.

**Strengths:** code generation, data analysis, content creation, visual
understanding, agentic tool use. Strong on autonomous coding agents, complex
agentic search, structured extraction and API-driven pipelines, code review,
interactive frontend and design tools.

**Effort:** `high` default. `xhigh` for the hardest coding and agentic tasks. `max`
for absolute maximum capability. `medium` for cost-sensitive. `low` only for short,
scoped, latency-sensitive tasks. Raise effort rather than prompting around a hard
problem.

**Constraints:**
- `temperature`, `top_p` and `top_k` return a **400 error**. Steer tone through the
  system prompt instead.
- Adaptive thinking is on by default. Leave `max_tokens` headroom for it at
  `high`/`xhigh`/`max`.
- Its tokenizer produces roughly 30% more tokens than Sonnet 4.6 for the same text
  — revisit any `max_tokens` inherited from Sonnet 4.6.

**Prompting:**
- Follows instructions literally; state scope explicitly.
- More agentic by default, but with thinking disabled it reaches for tools less —
  describe why and how to use each tool.
- For design work, specify concrete hexes and typefaces, or have it propose four
  distinct directions and pick one. Generic instructions like "make it clean" just
  shift it to a different fixed palette.

---

## Claude Haiku 4.5 — `claude-haiku-4-5`

**Price:** not listed in sources

Near-frontier performance, lightning fast and economical.

**Strengths:** real-time applications, high-volume intelligent processing,
cost-sensitive deployments, sub-agent tasks. Tracks its remaining context window.

**Prompting (inferred — no dedicated guide in sources):** give it one job with an
explicit output format and three to five examples. Do not hand it open-ended
multi-step autonomy.

---

## Effort ladder

`low` → `medium` → `high` → `xhigh` → `max`

Effort trades intelligence against latency and cost **within a single model**.
Before moving a unit of work up a model tier, check whether raising effort on the
current model closes the gap — it is usually cheaper.
