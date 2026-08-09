---
last_verified: 2026-08-09
expires: 2026-11-07
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

**Hard traps.** Prompt wordings and API settings that fail on this model are
listed once, in `canon/guardrails.md` under "Model-specific traps". Check there
before writing a prompt for it; they are deliberately not repeated here.
