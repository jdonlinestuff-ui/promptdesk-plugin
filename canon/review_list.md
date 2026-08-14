---
last_verified: 2026-08-13
expires: 2026-11-11
owner: chief-human-resource-officer
---

# Persona review checklist

The rubric `chief-human-resource-officer` applies when benchmarking any
`canon/personas/<name>.md` file. Distilled from the common shape of
published role-and-responsibility descriptions (role purpose → key
responsibilities → core functions → expectations/success measures →
required skills), cross-checked against this roster's own JD bench. No
external source is retained here — see the repo's no-third-party-URL rule.

Score each persona **Pass / Partial / Fail** per item, one line of reasoning
each. A **Fail** on items 1, 2 or 4 blocks the persona (it doesn't function
without them). A **Fail** on items 3, 5, 6, 7 or 9 is a gap to log, not a
blocker — most real-world role descriptions have the same gap.

1. **Role purpose stated.** One or two sentences on why this persona exists
   and what it's for — present in every persona file's opening line by
   design; check it's still specific to the work, not generic.

2. **Responsibilities are concrete, not generic.** Constraints name
   checkable behaviours ("quote the clause before characterising it"), not
   vague aspirations ("be accurate", "do good work").

3. **Success or evaluation criteria present.** Does the persona state how
   its own output should be judged — a deliverable's shape, a quality bar,
   a "done" condition — beyond just behavioural constraints? This is the
   most commonly missing item across both our roster and the real-world
   articles it's benchmarked against; a Fail here is expected on first pass,
   not a surprise.

4. **Required skills/competencies named.** The persona's suggested-skills
   list is current and specific to the work, not a generic catch-all.

5. **Escalation or authority boundary stated.** Does the persona say when it
   should stop and hand off — to a human, to outside expertise, to a
   different persona — rather than only naming when to switch model tier?
   (`legal`'s "recommend outside counsel" and `architect`'s stakeholder
   translation requirement are the current best examples; most personas
   don't have this yet.)

6. **Cross-functional dependency acknowledged.** Where the real-world role
   this persona is modeled on routinely depends on or hands off to another
   function, does the persona name that dependency, or does it read as if
   the work happens in isolation?

7. **Expectations measure value-add, not vanity.** Where item 3 passes,
   check the stated success criteria actually reflect whether the work
   helped — not an easy-to-game proxy. (Source methodology: choose metrics
   that measure real value-add, and match review cadence to how fast the
   underlying work actually changes.)

8. **Grounded, not invented.** Any claimed gap or addition traces to a real,
   citable job description or occupational profile — cite in conversation
   when doing the review; per house rule, don't retain the URL in the
   committed persona file.

9. **No third-party URL retained.** The persona file itself carries no
   external link. (`canon/personas/<name>.md` is committed to a public
   repo; `.privacy-guard/POLICY.md` governs this.)

## How to run a pass

1. Read the persona file.
2. Score all nine items.
3. Where 3, 5 or 6 fail, propose one concrete addition per failing item —
   phrased as a constraint, matching the persona's existing house style.
4. Report findings; do not write anything until the requester approves,
   per this persona's own standing constraint.
