---
name: assemble-team
description: >
  This skill should be used when the user asks to "put a team together",
  "assemble a team to review/design X", "get a few personas on this", "run
  this past a panel", "have the roster look at this", or otherwise wants
  several act-as personas to work a task at once rather than one persona
  handling it alone. Also use when a request spans more than one persona's
  territory and the work should fan out to specialists instead of one
  persona switching hats serially.
metadata:
  version: "0.1.0"
---

# Assemble team

Turn a review-or-design request into a small team of personas, each running
its own compliant, task-specific prompt, dispatched as subagents, then
synthesize what comes back.

## Step 1 — Read the roster, silently

Read `${CLAUDE_PLUGIN_ROOT}/canon/personas/INDEX.md`. Do not read every
persona file yet — only the ones that make the cast in Step 2.

## Step 2 — Decompose and cast the team

Restate the task in one sentence. If the restatement would surprise the
user, ask rather than proceed.

List the distinct angles the request actually needs — a design review might
call for `architect` + `reviewer` + `designer`; "review this idea" might
call for `product` + `financier` + `legal`, depending on what the idea
touches. Cast only the personas the task calls for. A team of one is a valid
outcome when the task doesn't actually fork; padding the roster for
appearances is not the goal.

For each cast persona, read its `${CLAUDE_PLUGIN_ROOT}/canon/personas/<name>.md`
file — role, model, effort, constraints, suggested skills.

If a needed angle has no matching persona, apply `act-as`'s own rule: never
silently substitute the nearest match. Name the gap and offer to either
build the persona (hand off to `act-as`'s interview) or proceed without that
seat.

## Step 3 — Draft each seat's prompt with prompt-author

For each cast persona, hand off to `prompt-author` to build a task-specific
system prompt: opens with the persona's role statement, folds in its
standing constraints as binding rules, and states this task's specific
deliverable and format. Have `prompt-author` check the draft against the
target model's traps in `${CLAUDE_PLUGIN_ROOT}/canon/guardrails.md` — a team
dispatch is exactly the multi-subagent case those traps exist for.

## Step 4 — Confirm the team with the requester

Before dispatching anything, show the requester:

- the cast — a table of persona, model, effort, and a one-line brief per seat
- each seat's drafted prompt

Ask whether the composition and prompts are right, or whether a seat should
be added, dropped, or reworded. Do not dispatch until the requester confirms
or explicitly says to proceed — this is the one gate this skill exists to
enforce, mirroring `act-as`'s escalation-confirmation habit and
`prompt-router`'s "ask the one clarifying question that most changes the
routing" rule.

## Step 5 — Route and dispatch

Once confirmed, run `prompt-router`'s procedure to finalize model and effort
per seat — persona defaults from `canon/personas/INDEX.md` are the starting
point, but this task's specific capability demand, latency, volume and
cost-of-error may still adjust them.

Dispatch each seat as an independent subagent running its confirmed prompt,
in parallel where seats don't depend on one another's output. Where a seat's
prompt genuinely depends on another seat's output (a reviewer commenting on
a design the architect hasn't produced yet), sequence it — forcing
independence onto a real dependency produces a seat with nothing to work
from.

## Step 6 — Synthesize and report back

Once every seat returns, do not just concatenate the outputs. Report:

- **Team:** table of persona, model, effort, seat brief.
- **Per-seat output:** each seat's finding or deliverable, attributed to
  that seat.
- **Where the team agreed or disagreed:** name real conflicts rather than
  averaging them away — a design review where the reviewer and the
  architect disagree is more useful than a silently smoothed verdict.
- **Open questions:** anything a seat flagged as unable to complete
  confidently.

## Rules

- Cast only the personas the task actually needs — a bigger team is not a
  better team.
- Never dispatch before the requester has seen and confirmed the cast and
  the prompts.
- Never silently substitute a near-match persona for a missing angle — name
  the gap, offer to build it.
- One subagent per persona per unit of work. A subagent wearing two
  personas' hats produces worse output than two focused calls — the same
  rule `prompt-router` applies to units of work in general.
- Sequence seats with a real dependency; don't parallelize past a genuine
  handoff.
- Attribute every synthesized finding to the seat that produced it.
