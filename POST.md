# Post copy

## GitHub description

OpenClaw × Codex bridge skill for task packets, runtime semantics, review/retry/reopen flow, and audit trace handling.

## Short post

I published `openclaw-codex-bridge`, a focused OpenClaw skill for treating Codex as an engineering execution layer instead of a generic chat model.

It covers:
- task packet structure
- ACP backend and permission profile selection
- completed / blocked / error result semantics
- review / retry / reopen flow
- audit tracing with `roomId`, `runId`, and `sourceRunId`

The goal is simple: make Codex collaboration more operational, traceable, and reusable inside OpenClaw workflows.

Repo: <REPO_URL>

## Chinese post

我刚把 `openclaw-codex-bridge` 整理成一个可公开复用的 OpenClaw skill 仓库。

它的定位不是“教你怎么跟 Codex 聊天”，而是把 Codex 当成**工程执行层**来协作，重点覆盖：

- task packet 任务包结构
- ACP backend / permission profile 选择
- completed / blocked / error 结果语义
- review / retry / reopen 流程
- roomId / runId / sourceRunId 审计追踪

核心目标是：
让 OpenClaw × Codex 的协作更像工程系统，而不是一次性 prompt。

仓库链接：<REPO_URL>
