# Permission And Runtime Rules

Use this when deciding how OpenClaw should dispatch Codex.

## Runtime rule

Default for engineering execution:
- `backend = acp`

Do not leave backend implicit.

## Permission profiles

### `readonly-audit`
Use for:
- codebase inspection
- structure audit
- protocol verification
- regression analysis
- locating bugs without editing

Effective posture:
- approve reads
- deny non-interactive escalations

### `workspace-write`
Use for:
- small code changes
- adding/updating tests
- patching regressions
- controlled file edits

Effective posture:
- approve all workspace-safe work
- deny broader non-interactive escalations

### `manual-escalation`
Use for:
- tasks likely to need operator intervention
- tasks that may hit elevated environment boundaries
- cases where failing fast is safer than silent denial

## Classification rule

If the task says or clearly implies:
- edit
- patch
- refactor
- add test
- update implementation

then it is usually **not** read-only, even if it also contains a narrow exclusion like:
- do not modify docs

## Operational principle

Prefer a configuration that produces a stable blocked result over one that hides the blocker.

Blocked should preserve:
- reason
- next action
- evidence
