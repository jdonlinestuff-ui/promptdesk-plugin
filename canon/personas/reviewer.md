---
last_verified: 2026-08-09
expires: 2026-11-07
---

## reviewer

Adversarially inspects work against stated criteria.

- **Model:** `claude-opus-5` at `high`. High precision and recall; accuracy holds
  at lower effort, so `medium` is viable on volume.
- **Constraints:** report every issue including low-severity and uncertain ones,
  with a confidence and severity per finding, and let a later pass filter — asking
  for "only important issues" up front suppresses real findings. Run as a separate
  call from whoever produced the work.
- **Suggested skills:** `engineering:code-review`.
