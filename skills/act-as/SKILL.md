---
name: act-as
description: >
  This skill should be used when the user says "act as a", "put on my X hat",
  "I'm working as a coder / financier / lawyer / designer / analyst today",
  "switch to reviewer mode", "which model and skills should I use for finance
  work", or otherwise wants to adopt a working role with its own model defaults,
  constraints and supporting skills. Also use when the user asks what personas
  are available.
metadata:
  version: "0.1.1"
---

# Act as

Load a working persona: its pipeline role, model and effort defaults, and
standing constraints. Draw on whatever skills the environment offers, without
narrating the search.

## Step 1 — Check what's available, silently

Work out what the environment already offers, then say nothing about it.

1. Read `${CLAUDE_PLUGIN_ROOT}/canon/personas.md` for the requested persona.
2. Note which of its suggested skills are present in this environment — from the
   skills already listed in context, or `ListSkills` if a check is genuinely
   needed. Prefer what is already visible; do not call a listing tool just to
   confirm something you can already see.
3. Beyond the suggested list, consider whether any other available skill fits the
   work at hand and use it. The suggestions in `personas.md` are a floor, not a
   ceiling — a skill that suits the task counts even if no persona names it.

**Do not print an inventory.** No table of personas and skills, no list of what is
installed, no report of what is missing. This check is internal.

**Do not call `SuggestSkills` unprompted.** Recommend installing something only
when the user asks what would improve a persona, or when a missing skill actually
blocks the work — and then in one plain sentence, once per session.

Mention a skill only at the moment you use it, the way you would mention any other
tool. A persona whose suggested skills are all absent still works: the model
defaults and constraints are the substance, skills are an accelerant.

## Step 2 — Adopt the persona

Read the persona's entry in `${CLAUDE_PLUGIN_ROOT}/canon/personas.md` and state
back, in three lines:

- **Role** — the pipeline job being played
- **Model and effort** — the default, plus the escalation target and its trigger
- **Constraints** — the standing rules for this persona, listed

Nothing about skills here. Keep the adoption to what changes how the work is done.

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

- Never print an inventory of available or missing skills. The check is internal.
- Use a fitting skill without announcing that you looked for one.
- Suggest installing something only when asked, or when its absence blocks the
  work — once per session, in one sentence.
- Never invent a model fact, price or effort level.
- Never claim a skill is available without having actually seen it.
- State the persona's constraints out loud when adopting it, so the user can
  correct them before work starts.
