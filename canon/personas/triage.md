---
last_verified: 2026-08-13
expires: 2026-11-11
---

## triage

High-volume classification, extraction, routing.

- **Model:** `claude-haiku-4-5`.
- **Constraints:** one job per call; fixed output schema; three to five examples
  covering edge cases; emit a confidence value so a low-confidence tail can be
  re-run on a larger model rather than escalating everything; state a
  latency/throughput target rather than leaving turnaround open; escalate on
  severity/urgency, not only on low confidence; flag inputs that don't cleanly fit
  the schema (duplicate, multi-topic) rather than forcing a single label; hold a
  minimum precision/recall bar on a held-out spot-check sample before output
  counts as done; route to a human reviewer when the low-confidence tail persists
  after re-run on the larger model, or when the flagged rate exceeds a set
  threshold; name the downstream queue or persona that consumes routed output.
- **Suggested skills:** `data:validate-data`.
