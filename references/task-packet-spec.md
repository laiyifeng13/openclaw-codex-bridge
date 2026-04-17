# Task Packet Spec

Use this when creating a packet for OpenClaw → Codex engineering dispatch.

## Packet version

Use:
- `openclaw.codex.dispatch.v1`

## Minimum structure

```json
{
  "packetVersion": "openclaw.codex.dispatch.v1",
  "backend": "acp",
  "request": { ... },
  "task": { ... }
}
```

## Required authoring rules

### 1. Make backend explicit

Always set:
- `backend = acp`

Do not rely on global runtime defaults.

### 2. Write requirements as executable work

Good requirements are concrete and testable.

Good:
- add structured audit filtering for `roomId`, `runId`, `sourceRunId`
- update the collaboration batch results row to deep-link to audit trace
- add regression coverage for the new audit filters

Weak:
- improve the audit page
- make Codex handle this better

### 3. Use constraints for guardrails

Put these in constraints:
- forbidden files/areas
- do-not-touch scopes
- test scope limits
- output shape requirements
- permission boundaries

Examples:
- do not modify docs in this run
- do not widen scope beyond the audit timeline and review queue UI
- run only the focused regression tests listed below

### 4. Definition of done must be concrete

A strong `definitionOfDone` should name:
- behavior change
- validation method
- any file/test expectations

Example:
- `/audit` and `/api/audit` accept read-only `roomId/runId/sourceRunId` filters
- batch results rows expose `Open audit` links with stable trace identity
- focused regression passes

### 5. Provide artifact anchors when possible

If you already know likely touchpoints, include them.

Examples:
- `src/runtime/audit-timeline.ts`
- `src/ui/server.ts`
- `test/audit-timeline.test.ts`

## Recommended fixed constraints

Reuse these often:
- keep the scope minimal and avoid unrelated refactors
- preserve existing behavior outside the named files
- explain blocked conditions explicitly if the task cannot complete
- return changed files and verification commands

## Delivery shape to request from Codex

Ask for:
1. what changed
2. why
3. verification run
4. remaining risks
5. next step

## Starter template

Use `../assets/packet.template.json` as the base.
