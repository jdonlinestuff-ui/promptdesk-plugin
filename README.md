# promptdesk

Routes work to the right Claude model and role, writes prompts to documented
Anthropic best practice, and loads working personas with their own model defaults
and constraints.

Self-contained. No external services, no MCP servers, no credentials, no
dependency on any other plugin. Skills from the Claude store are suggested where
they help and skipped silently where they are absent.

## What it does

| Skill | Triggers on | Produces |
|---|---|---|
| `prompt-router` | "which model should I use", "route this task", "should this be Haiku or Sonnet", "what effort level" | A model, effort and role per unit of work, plus a ready-to-paste system prompt for each |
| `prompt-author` | "write a prompt", "review my prompt", "why isn't this prompt working", "lint this" | A prompt built to the ten-component architecture, or a numbered findings table with severities and fixes |
| `act-as` | "act as a financier", "put on my coder hat", "switch to reviewer mode" | Persona adopted: role, model and effort defaults, standing constraints, and which supporting skills are installed |

## Personas

`coder`, `architect`, `reviewer`, `debugger`, `agent-runner`, `financier`,
`legal`, `product`, `marketer`, `designer`, `analyst`, `researcher`, `triage`,
`writer`, `facilitator`.

Each carries a default model and effort, an escalation target with a named
trigger, standing constraints, and suggested store skills.

## First run

`act-as` inventories the skills available in the environment, maps them against
each persona, and reports what is present and what is missing. Missing skills are
offered once as suggestions from the Claude store. Nothing blocks on them —
a persona's model defaults and constraints work regardless.

## Canon

All model facts and prompting rules live in `canon/`, shared by the three skills
so they cannot drift apart:

| File | Holds |
|---|---|
| `model-reference.md` | Model IDs, prices, specs, effort ladders, per-model prompting rules and constraints |
| `best-practices.md` | General prompting guidance |
| `prompt-architecture.md` | The ten-component prompt structure |
| `guardrails.md` | Hallucination and consistency techniques, plus per-model traps |
| `personas.md` | Persona definitions |
| `SOURCES.md` | Every source URL, what it governs, and known gaps |

Each canon file carries a `last_verified` date. When a file is more than 90 days
old, the skills warn before relying on it and point at `SOURCES.md` for
re-verification. Model facts go stale — this is the mechanism that surfaces it
rather than letting an outdated table quietly become the source of truth.

## Adding or changing a persona

Personas are plain markdown entries in `canon/personas.md`. Add a heading, a model
default, an escalation trigger, constraints and suggested skills. No code change,
no rebuild of the skills.

## Setup

None. Install and use.

## Known gaps

- No dedicated prompting guide exists for Claude Haiku 4.5 in the source set; its
  entry is marked as inferred from the general guidance.
- The model-selection source lists no pricing for Sonnet 5 or Haiku 4.5.
