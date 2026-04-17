---
name: openclaw-codex-bridge
description: Route OpenClaw engineering work to Codex as an ACP coding agent. Use when creating or reviewing Codex task packets, choosing ACP permission profiles, interpreting completed/blocked/error runtime results, handling review/retry/reopen flow, or tracing roomId/runId/sourceRunId audit evidence.
---

# openclaw-codex-bridge

Use this skill when OpenClaw should treat Codex as a **coding execution layer**, not a general chat model.

## What this skill is for

Use it to:
- prepare a Codex dispatch packet
- choose the right ACP permission profile
- interpret runtime results (`completed` / `blocked` / `error`)
- drive review / retry / reopen flow
- inspect audit trace with `roomId`, `runId`, `sourceRunId`
- standardize delivery format for engineering tasks

Do **not** use it for:
- general model/provider selection
- non-Codex chat prompting
- long-term project planning without a concrete coding dispatch

## Core operating rules

1. **Codex is the execution layer.** OpenClaw keeps orchestration, memory, messaging, and task decomposition.
2. **Always make backend explicit.** Prefer `backend = acp`; never rely on silent runtime defaults.
3. **Permission profile must match task type.**
   - read-only audit/check tasks → `readonly-audit`
   - writable code/doc tasks → `workspace-write`
   - manual escalation scenarios → `manual-escalation`
4. **Blocked is recoverable state, not noise.** Preserve blocker, next action, and evidence.
5. **Latest `.report.json` is authoritative.** Older blocked reports are supplemental only and may be stale.
6. **Trace identity priority is fixed.** `runId` is primary; `sourceRunId` is supplemental context only.

## Workflow

### 1) Build the task packet

Read `references/task-packet-spec.md` and use `assets/packet.template.json`.

Minimum expectations:
- explicit `packetVersion`
- explicit `backend`
- concrete `requirements`
- concrete `constraints`
- strong `definitionOfDone`
- artifact anchors when available

### 2) Choose the permission profile

Read `references/permission-and-runtime-rules.md`.

Quick rule:
- inspect / inventory / protocol-check / regression-check → `readonly-audit`
- edit / patch / refactor / add tests / change files → `workspace-write`

If uncertain, prefer a profile that allows a stable blocked result rather than hiding the root cause.

### 3) Run and interpret results

Read `references/result-semantics.md`.

Minimum interpretation:
- `completed` → move to review / verification
- `blocked` → write blocked report, do not blind-retry
- `error` or missing structured result → treat as runtime failure and preserve evidence

### 4) Handle review / retry / reopen

Read `references/review-retry-reopen.md`.

Use:
- **review** when a run completed and needs approval/rejection
- **retry** when the same task should continue as a new run with lineage
- **reopen** when a task should be restarted as a fresh workflow step

### 5) Inspect audit trace

Read `references/audit-trace.md`.

Use trace filters structurally:
- `roomId`
- `runId`
- `sourceRunId`

Priority rule:
- if `runId` exists, trace by `runId`
- use `sourceRunId` only as supplemental context, or only when `runId` is absent

## Fixed output contract for Codex work

Ask Codex to return, in order:
1. what changed
2. why it changed
3. what was verified
4. current risks / remaining gaps
5. suggested next step

Keep a structured result block at the end when the runtime expects it.

## Files in this skill

- `references/task-packet-spec.md` — packet schema and task-writing rules
- `references/permission-and-runtime-rules.md` — ACP backend/profile rules
- `references/result-semantics.md` — completed/blocked/error interpretation
- `references/review-retry-reopen.md` — run lineage workflow
- `references/audit-trace.md` — trace filters and empty-hit interpretation
- `assets/packet.template.json` — starter packet template
- `assets/blocked-report.template.json` — starter blocked report template

## Practical default

If the user says something like:
- “把这个工程任务交给 codex”
- “给 codex 发一个可执行的代码任务”
- “看下这个 blocked report / audit trace”
- “继续上一个 run，或者 retry / reopen”

then:
1. load the relevant reference file(s)
2. make backend/profile explicit
3. produce or inspect structured packet/report/audit data
