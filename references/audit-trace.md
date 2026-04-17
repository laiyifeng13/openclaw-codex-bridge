# Audit Trace

Use this when inspecting why a Codex task is in its current state or when deep-linking from batch results into audit.

## Trace fields

Use these filters structurally:
- `roomId`
- `runId`
- `sourceRunId`

## Identity priority

This rule is fixed:
- `runId` is the primary trace identity
- `sourceRunId` is supplemental context only

So:
- if `runId` exists, trace by `runId`
- only use `sourceRunId` as the main narrowing key when `runId` is absent

## Typical deep-link shape

```text
/audit?roomId=<roomId>&runId=<runId>&sourceRunId=<sourceRunId>
```

## Empty-hit interpretation

If audit returns no events, explain the current trace filter and likely reasons, for example:
- the run has no persisted audit entries yet
- the trace identity is stale
- the event only exists as supplemental context

Do not treat an empty hit as proof that nothing happened.

## What batch results should do

Per-item batch result rows should ideally expose:
- `Go handle`
- `Open audit`

Where `Open audit` uses stable trace identity from the per-item run, not a synthetic batch-level audit object.
