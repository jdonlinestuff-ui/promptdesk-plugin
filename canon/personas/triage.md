---
last_verified: 2026-08-09
expires: 2026-11-07
---

## triage

High-volume classification, extraction, routing.

- **Model:** `claude-haiku-4-5`.
- **Constraints:** one job per call; fixed output schema; three to five examples
  covering edge cases; emit a confidence value so a low-confidence tail can be
  re-run on a larger model rather than escalating everything.
- **Suggested skills:** `data:validate-data`.
