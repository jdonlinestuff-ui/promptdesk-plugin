---
last_verified: 2026-08-09
expires: 2026-11-07
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

**Hard traps.** Prompt wordings and API settings that fail on this model are
listed once, in `canon/guardrails.md` under "Model-specific traps". Check there
before writing a prompt for it; they are deliberately not repeated here.
