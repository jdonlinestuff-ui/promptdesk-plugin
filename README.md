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
| `assemble-team` | "put a team together", "assemble a team to review/design X", "run this past a panel", "have the roster look at this" | A cast of personas with drafted, confirmed prompts, dispatched as subagents, and a synthesized report attributing each finding to its seat |

## Personas

`coder`, `architect`, `reviewer`, `debugger`, `agent-runner`, `financier`,
`legal`, `product`, `marketer`, `designer`, `analyst`, `researcher`, `triage`,
`writer`, `facilitator`, `chief-human-resource-officer`.

Each carries a default model and effort, an escalation target with a named
trigger, standing constraints, and suggested store skills. One file per persona
under `canon/personas/`, so adopting one reads one file rather than all fifteen.

Ask for a persona that doesn't exist and `act-as` offers to build it instead of
quietly using the nearest match: a four-question interview covering what it
produces, which existing persona to inherit from, its always/never constraints,
and any web pages or documents it should follow. References are fetched and
distilled into constraints with sources and a verification date. A persona built
this way lasts the session; keeping it means adding the file to the plugin source
and reinstalling.

## Skill awareness

`act-as` checks what the environment already offers and uses whatever fits — the
skills named per persona are a floor, not a ceiling. This check is silent: no
inventory table, no list of what's installed, no unprompted install suggestions.
A skill gets mentioned when it is used, and nothing blocks on one being absent.

Ask what would improve a persona and it will tell you. Otherwise it stays quiet.

## Canon

All model facts and prompting rules live in `canon/`, shared by the three skills
so they cannot drift apart:

| File | Holds |
|---|---|
| `models/INDEX.md` | Selection table, effort ladder, access and compliance filters |
| `models/<model-id>.md` | One file per model: specs, strengths, effort defaults, prompting rules |
| `personas/INDEX.md` | Persona table — name, role, model, effort |
| `personas/<name>.md` | One file per persona |
| `best-practices.md` | General prompting guidance |
| `prompt-architecture.md` | The ten-component prompt structure |
| `guardrails.md` | Hallucination and consistency techniques, and the **only** copy of the per-model traps |
| `SOURCES.md` | Every source URL, what it governs, and known gaps |

Everything is split so a skill reads what it needs and nothing else — routing
between two models reads two model files, not six; adopting one persona reads one
file, not fifteen.

The per-model traps live in exactly one place. Model files point at
`guardrails.md` rather than restating them, because a rule stored twice eventually
disagrees with itself.

Each canon file carries `last_verified` and an explicit `expires` date. On or
after `expires`, the skills warn before relying on the file and point at
`SOURCES.md` for re-verification. Model facts go stale — this surfaces it instead
of letting an outdated table quietly become the source of truth.

## Adding or changing a persona

Add `canon/personas/<name>.md` — frontmatter, role, model and effort, escalation
trigger, constraints, suggested skills — and a row in `canon/personas/INDEX.md`.
No code change, no rebuild of the skills. Or let `act-as` interview you and write
it.

## Setup

None. Install and use.

## Known gaps

- No dedicated prompting guide exists for Claude Haiku 4.5 in the source set; its
  entry is marked as inferred from the general guidance.
- The model-selection source lists no pricing for Sonnet 5 or Haiku 4.5.
