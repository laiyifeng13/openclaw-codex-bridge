# Result Semantics

Use this when interpreting runtime output from Codex dispatch.

## Authoritative result rule

Treat the latest `.report.json` as the source of truth.

A `.blocked-report.json` is supporting evidence only.
If a newer successful run exists, an older blocked report may be stale and must not override the latest authoritative result.

## Core statuses

### `completed`
Meaning:
- runtime completed the requested work
- next step is usually review / verification

Expected follow-up:
- inspect changed files
- run the stated verification command if needed
- submit for review / approve or reject

### `blocked`
Meaning:
- runtime did not finish, but returned a recoverable blocker

Expected follow-up:
- write or preserve a blocked report
- do not blind-retry the same packet
- act on `nextAction`

### `error`
Meaning:
- runtime failed before forming a stable acceptable result
- or returned an explicit response error

Expected follow-up:
- preserve stderr/stdout and runtime evidence
- treat as runtime failure, not business success

## Stable fields to preserve when available

- `authoritativeStatus`
- `sourceOfTruth`
- `reportStatus`
- `blockedReportStatus`
- `blockedReason`
- `explanation`
- `nextAction`

## Minimum acceptance for a good completed result

A production-usable completed result should include:
- at least one exact verification command
- a pass/fail claim that can be checked
- artifact refs for touched code/tests
