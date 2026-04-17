# Review Retry Reopen

Use this when a Codex run is already in the workflow and needs a next-step decision.

## Review

Use review when a run is completed and should be judged.

Possible decisions:
- approve
- reject

Approval effect:
- confirms the run as accepted
- can move room/task to completed/done depending on the workflow

Reject effect:
- marks the run as not accepted
- often leads to retry or reopen depending on whether lineage should continue

## Retry

Use retry when:
- the same task should continue as a new run
- you want lineage from an older run to a newer run
- the prior run is still the right task context, but needs another pass

Trace rule:
- new run gets its own `runId`
- old run becomes `sourceRunId`
- `runId` is primary; `sourceRunId` is context only

## Reopen

Use reopen when:
- the task should restart as a fresh workflow step
- the previous run lineage is no longer the right main context
- operator wants a clean re-entry into execution

## Minimal operator heuristic

- approve if result is acceptable and verification is sufficient
- reject if result is wrong/incomplete but the same task intent still holds
- retry if you want a new run from the same task lineage
- reopen if the workflow should restart more cleanly than retry
