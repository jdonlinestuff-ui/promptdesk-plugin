---
last_verified: 2026-08-09
expires: 2026-11-07
---

# Sources

Every model fact and prompting rule in `canon/` traces to one of these pages.
Nothing else is canon. Third-party guides, blog posts and remembered figures are
not sources — if a claim cannot be traced to this list, it does not go in.

`last_verified` on each canon file records when its contents were last checked
against these pages.

## Model facts

| Governs | URL |
|---|---|
| Model list, selection framework, effort | https://platform.claude.com/docs/en/about-claude/models/choosing-a-model |
| Fable 5 / Mythos 5 specs, access, refusals | https://platform.claude.com/docs/en/about-claude/models/introducing-claude-fable-5-and-claude-mythos-5 |

## Prompting rules

| Governs | URL |
|---|---|
| General best practice | https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices |
| Fable 5 specifics | https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5 |
| Opus 5 specifics | https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5 |
| Opus 4.8 specifics | https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-4-8 |
| Sonnet 5 specifics | https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-sonnet-5 |

## Guardrails

| Governs | URL |
|---|---|
| Hallucination reduction | https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations |
| Output consistency | https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/increase-consistency |

## Known gaps

- No dedicated prompting guide exists for Claude Haiku 4.5 among these sources.
  Its entry in `models/claude-haiku-4-5.md` derives from the general
  best-practices page and is marked as inferred.
- The model-selection page lists no pricing for Sonnet 5 or Haiku 4.5.

## Re-verification

When any canon file's `last_verified` is more than 90 days old, re-read the pages
above that govern it, update the file, and reset the date. Do not update the date
without re-reading.
