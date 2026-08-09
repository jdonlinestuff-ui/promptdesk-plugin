---
name: act-as
description: >
  This skill should be used when the user says "act as a", "put on my X hat",
  "I'm working as a coder / financier / lawyer / designer / analyst today",
  "switch to reviewer mode", "which model and skills should I use for finance
  work", or otherwise wants to adopt a working role with its own model defaults,
  constraints and supporting skills. Also use on first install, or when the user
  asks what personas are available or what skills would improve a persona.
metadata:
  version: "0.1.0"
---

# Act as

Load a working persona: its pipeline role, model and effort defaults, standing
constraints, and the installed skills that support it.

## Step 1 — Inventory the environment

Run this once per session, before adopting a persona, and always on the first
invocation after install.

1. List the skills available in this environment (use `ListSkills`, or the
   equivalent listing this environment provides).
2. Read `${CLAUDE_PLUGIN_ROOT}/canon/personas.md`.
3. Match each persona's suggested skills against what is actually installed.
4. Record the result for the rest of the session so the inventory runs once.

Report the inventory compactly — a table of persona, skills found, skills missing.
Do not dump the full skill list.

If skills are missing for personas the user actually uses, offer them once via
`SuggestSkills` (or name them in plain text if that tool is unavailable), then
continue. Never block on a missing skill and never suggest the same gap twice in a
session.

If a persona's suggested skills are all absent, the persona still works — the
model defaults and constraints are the substance; skills are an accelerant.

## Step 2 — Adopt the persona

Read the persona's entry in `${CLAUDE_PLUGIN_ROOT}/canon/personas.md` and state
back, in four lines:

- **Role** — the pipeline job being played
- **Model and effort** — the default, plus the escalation target and its trigger
- **Constraints** — the standing rules for this persona, listed
- **Skills active** — installed ones that will be used; missing ones named once

Then apply the constraints for the rest of the session, or until the user switches
persona. Treat them as binding, not advisory.

## Step 3 — Handle the work

Work as that persona. When the request spans more than one persona's territory,
say so and either switch explicitly or hand off to `prompt-router` to split the
work across models and roles.

When the persona's escalation trigger fires — for example `financier` producing a
multi-source board pack rather than a variance check — name the trigger, name the
model being escalated to, and confirm before switching if the cost difference is
material.

## Available personas

`coder`, `architect`, `reviewer`, `debugger`, `agent-runner`, `financier`,
`legal`, `product`, `marketer`, `designer`, `analyst`, `researcher`, `triage`,
`writer`, `facilitator`.

Full definitions in `${CLAUDE_PLUGIN_ROOT}/canon/personas.md`.

If the user names a persona that does not exist, do not force the nearest match
silently. Name the closest two, say what each is for, and let them choose. If none
fit, work without a persona and offer to add one — persona definitions are plain
markdown entries, so a new one is a small addition rather than a rebuild.

## Model facts

Model IDs, effort levels, capabilities and constraints come only from
`${CLAUDE_PLUGIN_ROOT}/canon/model-reference.md`. Check its `last_verified` date;
if today is more than 90 days later, say the facts may be stale and point at
`${CLAUDE_PLUGIN_ROOT}/canon/SOURCES.md`.

Model defaults are starting points. When the user reports that a persona's default
is consistently too weak or too expensive for their work, say plainly that the
default should be changed in `personas.md` rather than worked around each session.

## Rules

- Inventory once per session, not once per request.
- Suggest a missing skill once, then drop it.
- Never invent a model fact, price or effort level.
- Never claim a skill is installed without having seen it in the inventory.
- State the persona's constraints out loud when adopting it, so the user can
  correct them before work starts.
