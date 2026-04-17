# openclaw-codex-bridge

OpenClaw × Codex bridge skill for task packets, runtime semantics, review / retry / reopen flow, and audit trace handling.

## What it is

`openclaw-codex-bridge` is a reusable OpenClaw skill that treats Codex as an engineering execution layer instead of a generic chat model.

It focuses on the operational bridge between OpenClaw orchestration and Codex execution, including:

- task packet authoring
- ACP backend and permission profile selection
- completed / blocked / error runtime result interpretation
- review / retry / reopen flow
- audit tracing with `roomId`, `runId`, and `sourceRunId`
- standardized delivery format for engineering tasks

## Core idea

OpenClaw keeps orchestration, memory, messaging, and task decomposition.

Codex handles execution:

- writing code
- patching files
- running verification
- returning structured results

The key is not just prompting better. The key is creating a repeatable engineering protocol around:

- explicit task packets
- explicit permission profiles
- blocked-state recovery
- review / retry / reopen lineage
- audit trace evidence

## Repository structure

- `SKILL.md` — skill definition and operating rules
- `references/` — protocol docs and execution guidance
- `assets/packet.template.json` — starter task packet template
- `assets/blocked-report.template.json` — starter blocked report template
- `_meta.json` — lightweight repo metadata
- `skill-repo.json` — packaging note for OpenClaw skill distribution

## Included references

- `references/task-packet-spec.md`
- `references/permission-and-runtime-rules.md`
- `references/result-semantics.md`
- `references/review-retry-reopen.md`
- `references/audit-trace.md`

## Use cases

Use this skill when you need to:

- dispatch a concrete engineering task to Codex
- choose the right ACP execution profile
- inspect a blocked run and preserve evidence
- continue work via retry or reopen
- trace execution lineage across runs

## Status

Current version: `0.1.0`

This repository is intentionally minimal so it can be published cleanly as a focused skill repo.
