# Source of Truth Map

Status: CANONICAL  
Stage: MRD_SUPPORT

## 1. 为什么要有这份文档

这个项目已经发生过一次典型混乱：

- 历史聊天里生成过一些文件
- 后来这些文件被迁移、重命名、归档
- 新 repo 又开始重新写正式文档

如果不明确“哪类文件算当前真相”，
后面就会反复出现下面的问题：

1. 不知道该看哪份文档
2. 不知道 imported 里的旧文件还能不能直接沿用
3. 不知道 archive 里的聊天和会话记录是什么身份

所以这份文档只做一件事：

> 明确当前 repo 里，什么是 canonical source of truth，什么只是证据材料。

## 2. Canonical Layer

当前 repo 里，唯一的 canonical layer 是：

`docs/`

也就是说：

- 当前产品定义，以 `docs/` 为准
- 当前阶段状态，以 `docs/` 为准
- 当前 workflow 与 phase 语义，以 `docs/` 为准

即使 imported 或 archive 里出现不同说法，
也不能自动覆盖 `docs/`。

## 3. Evidence Layer

下面两类路径属于 evidence layer，不是当前真相层：

1. `imported/`
2. `archive/`

它们的作用分别是：

### imported

承载从旧 repo / 旧上下文迁过来的有效材料。

### archive

承载本地 session、日志、thread transcript、检索索引等归档证据。

## 4. Current Canonical Documents

当前 `docs/` 下已经建立的 canonical 文档包括：

1. `global-goal.md`
2. `requirements-clarification.md`
3. `development-workflow.md`
4. `development-step-breakdown.md`
5. `task-breakdown.md`
6. `tech-stack-options.md`
7. `mrd-prd-relationship.md`
8. `mrd.md`
9. `prd.md`
10. `mainline-status.md`
11. `current-clarification.md`
12. `product-boundary-v1.md`
13. `conversation-state-machine-v1.md`
14. `source-of-truth-map.md`

## 5. Imported Materials That Still Matter

下面这些 imported 文档仍然重要，
但身份是“上游证据”，不是当前直接工作文档：

1. `imported/non-drift-communication-protocol/01_protocol/boundary-confirmation-v1.md`
2. `imported/non-drift-communication-protocol/01_protocol/mainline-architecture-v1.md`
3. `imported/evolving-memory-system/04_protocols/global-inscope-lock-protocol-v1.md`
4. `imported/evolving-memory-system/04_protocols/thread-kickoff-template-v1.md`

它们贡献的核心价值分别是：

1. 锁语义
2. 主线路命名与治理意识
3. Goal / Scope / Next / Drift 的骨架
4. kickoff 模板思路

## 6. Archive Materials That Matter

下面这些 archive 材料对追溯有价值：

1. `archive/thread-1499796593535221961/derived/thread-transcript-local.md`
2. `archive/thread-1499796593535221961/derived/frameworks/framework-inventory.md`
3. `archive/thread-1499796593535221961/derived/indexes/related-files-index.md`

它们的作用是：

1. 证明之前聊过什么
2. 帮当前 repo 找回历史上下文
3. 作为后续整理或校验依据

但它们不直接决定当前阶段应该怎么推进。

## 7. Conflict Resolution Rule

如果不同层之间出现冲突，优先级固定为：

1. `docs/`
2. `imported/`
3. `archive/`

换句话说：

- `docs/` 负责当前
- `imported/` 负责承接
- `archive/` 负责举证

## 8. 使用规则

后面如果再整理历史材料，统一按下面规则处理：

1. 先从 `imported/` 或 `archive/` 找证据
2. 再把真正通过的内容收口进 `docs/`
3. 不直接把 imported 文档当成当前工作文档
4. 不直接把 archive transcript 当成规范

## 9. 当前建议

从现在开始，任何“前面聊过的内容要不要继续沿用”，
都先问两个问题：

1. 它有没有已经进入 `docs/`
2. 如果还没进入，应该进入哪一份 canonical 文档

这样后面项目继续推进时，就不会再反复回到“之前是不是聊过这个、文件到底在哪”。
