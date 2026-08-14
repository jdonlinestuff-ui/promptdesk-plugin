---
last_verified: 2026-08-09
expires: 2026-11-07
---

# Persona index

Pick one, then read only `canon/personas/<name>.md`.
Do not read every persona file to use one.

| Persona | Role | Model | Effort |
|---|---|---|---|
| `coder` | Implementer | `claude-sonnet-5` | high |
| `architect` | Architect | `claude-opus-5` | xhigh |
| `reviewer` | Reviewer | `claude-opus-5` | high |
| `debugger` | Diagnostician | `claude-sonnet-5` | high |
| `agent-runner` | Orchestrator | `claude-fable-5` | high |
| `financier` | Analyst | `claude-opus-5` | high |
| `legal` | Reviewer | `claude-opus-5` | high |
| `product` | Drafter | `claude-sonnet-5` | high |
| `marketer` | Drafter | `claude-sonnet-5` | medium |
| `designer` | Designer | `claude-opus-4-8` | xhigh |
| `analyst` | Analyst | `claude-sonnet-5` | high |
| `researcher` | Researcher | `claude-opus-5` | high |
| `triage` | Classifier | `claude-haiku-4-5` | — |
| `writer` | Drafter | `claude-sonnet-5` | high |
| `facilitator` | Synthesiser | `claude-sonnet-5` | medium |
| `chief-human-resource-officer` | Roster curator | `claude-opus-5` | high |

Model defaults are starting points, not verdicts. Benchmarking on the user's
own prompts and data is the deciding step.

Suggested skills inside each persona file are a floor, not a ceiling: use any
available skill that fits, and never block on one being absent.

Adding a persona means adding one file here plus a row in this table.

A name that isn't in this table is not automatically a mistake — it often means
the work needs something these fifteen don't cover. Offer to build it rather than
substituting the nearest match. `act-as` holds the interview and the file shape.
