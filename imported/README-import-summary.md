# Communication Governance 历史资料迁移摘要

## 1) 你提到的关键协议文件（已迁移）
- `imported/evolving-memory-system/04_protocols/global-inscope-lock-protocol-v1.md`
- `imported/evolving-memory-system/04_protocols/thread-kickoff-template-v1.md`
- `imported/non-drift-communication-protocol/01_protocol/boundary-confirmation-v1.md`
- `imported/non-drift-communication-protocol/01_protocol/mainline-architecture-v1.md`

迁移清单：`imported/migration-manifest.json`

## 2) 聊天记录固化位置（系统原始落盘）
- Hermes 会话 JSON：`~/.hermes/sessions/`
- 会话数据库：`~/.hermes/hermes.db`
- 网关日志：`~/.hermes/logs/gateway.log`、`~/.hermes/logs/agent.log`

## 3) 与当前 thread 相关的会话副本（已复制到本仓库）
- `imported/hermes-sessions/session_20260501_101047_50c2572f.json`
- `imported/hermes-sessions/session_20260501_230437_5911e9.json`
- `imported/hermes-sessions/session_20260501_233614_40ce8388.json`
- `imported/hermes-sessions/session_20260502_002118_16ca95.json`
- `imported/hermes-sessions/session_20260502_005440_4d47bb.json`
- `imported/hermes-sessions/session_20260502_010033_a86e26.json`

> 注：会话文件是“复制”而不是“移动”，避免破坏 Hermes 原有索引。

## 4) 你截图里提到但当前磁盘未找到的文件
- `lock-state.schema.json`
- `lock_state.json`
- `channel_map.json`
- `drift_guard.py`
- `kickoff.sh`

目前在 `/Users/chao/projects` 范围内未检索到这些文件名。