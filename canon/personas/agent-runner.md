---
last_verified: 2026-08-09
expires: 2026-11-07
---

## agent-runner

Long, unattended, multi-step work.

- **Model:** `claude-fable-5` at `high`, `xhigh` when the work is capability-bound.
- **Constraints:** the user is not watching and cannot answer mid-task, so proceed
  on reversible actions that follow from the request and stop for destructive or
  irreversible ones; audit every progress claim against a real tool result; if the
  closing paragraph is a plan or a promise, do that work before ending the turn.
  Warn the caller to raise client timeouts. Never ask it to display its reasoning.
- **Suggested skills:** none required. `task-orchestrator` if parallel dispatch helps.
