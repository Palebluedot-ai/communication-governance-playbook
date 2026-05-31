# PRD

Status: DRAFT  
Stage: PRD  
Depends on:

- `docs/mrd.md`
- `docs/task-breakdown.md`
- `docs/tech-stack-options.md`

## 1. Product Scope

这个 `PRD` 描述的是第一版产品要求，不是长期愿景全集。

第一版只聚焦一件事：

> 把 `Global Goal -> 需求澄清 -> 开发任务拆解 -> MRD -> PRD -> BMaD 开发`
> 做成一个强约束 workflow engine。

## 2. Product Goal

第一版产品目标不是“自动帮用户写完所有文档”，而是：

- 明确当前阶段
- 明确当前阶段所需文档
- 阻断非法切阶段
- 记录批准和阻断原因
- 让用户始终知道当前允许做什么

## 3. Target User

### Primary User

- 单 owner
- 单 repo
- 和 AI 一起做产品或研发编排的人

### User Traits

- 有全局目标
- 会追问细节
- 经常产生新想法
- 不希望 AI 顺着发散自动换阶段

## 4. Core User Jobs

用户需要系统帮助完成下面这些工作：

1. 锁定当前全局目标
2. 开始某个阶段
3. 查看当前阶段状态
4. 审批当前阶段完成
5. 进入下一个阶段
6. 被阻断时知道为什么被阻断
7. 冷启动时快速恢复当前项目状态

## 5. Core Objects

第一版必须明确下面这些核心对象：

### 5.1 Phase

表示当前处于哪一个 workflow 阶段。

第一版 phase 固定为：

- `goal`
- `clarification`
- `task_breakdown`
- `mrd`
- `prd`
- `bmad`

### 5.2 Phase Status

表示某个阶段目前的状态。

第一版至少支持：

- `not_started`
- `in_progress`
- `approved`
- `blocked`

### 5.3 Document Contract

表示每个阶段必须绑定的文档及其最低要求。

### 5.4 Approval Record

表示某个阶段何时被谁批准。

### 5.5 Block Reason

表示为什么不能进入下一阶段。

## 6. Workflow Requirements

### 6.1 Required Phase Order

系统必须按下面顺序推进：

`goal -> clarification -> task_breakdown -> mrd -> prd -> bmad`

### 6.2 No Implicit Transition

系统不得因为以下行为自动切阶段：

- 用户追问细节
- 用户补充背景
- 用户临时冒出一个 idea
- AI 自己提出“下一步不如做 X”

### 6.3 Explicit Approval Required

从当前阶段进入下一阶段前，必须满足：

1. 当前阶段文档存在
2. 当前阶段文档达到最低契约
3. 当前阶段被显式批准

### 6.4 Status Visibility

系统必须能明确展示：

- 当前 phase
- 当前 phase status
- 当前绑定文档
- 下一步允许动作
- 当前阻断原因

## 7. Transition Requirements

### 7.1 Allowed Transitions

第一版只允许下面这些前进式切换：

- `goal -> clarification`
- `clarification -> task_breakdown`
- `task_breakdown -> mrd`
- `mrd -> prd`
- `prd -> bmad`

### 7.2 Illegal Transitions

第一版至少要阻断：

- 跳级切换
- 当前阶段未批准就切换
- 缺失必需文档就切换
- 在 `clarification` 阶段把普通追问误当成切换

### 7.3 Explainability

每次阻断都必须返回：

- reason code
- 一句可读解释
- 当前允许动作

## 8. Document Contract Requirements

### 8.1 Clarification Document

至少要有：

- 项目背景
- 当前痛点
- 一句话需求
- 当前明确不做的事

### 8.2 Task Breakdown Document

至少要有：

- 任务块列表
- 第一版范围
- 推荐顺序
- 进入 MRD 的条件

### 8.3 MRD Document

至少要有：

- 问题定义
- 目标用户
- 价值主张
- 第一版范围
- 成功标准

### 8.4 PRD Document

至少要有：

- workflow 定义
- 核心对象
- transition 规则
- command contract
- block 规则
- 验收标准

## 9. CLI Command Requirements

### 9.1 First Command Set

第一版至少需要：

- `goal lock`
- `phase show`
- `clarification start`
- `clarification approve`
- `task-breakdown start`
- `task-breakdown approve`
- `mrd start`
- `mrd approve`
- `prd start`
- `prd approve`
- `bmad start`
- `transition explain`

### 9.2 Command Behavior

每个命令至少要有：

- 输入校验
- 成功输出
- 失败输出
- reason code

### 9.3 Guarded Entry

像 `bmad start` 这种命令必须是 guarded command：

- 如果前置条件不满足，直接 block
- 不能“尽量帮忙继续”

## 10. Storage Requirements

### 10.1 Markdown

用于保存阶段文档和人类可读说明。

### 10.2 JSON

用于保存 workflow 配置和 phase 规则。

### 10.3 SQLite

用于保存：

- 当前 phase
- phase status
- approval history
- block history
- 当前 next action

## 11. Guard Requirements

第一版 guard 至少要覆盖：

1. 非法 phase transition
2. 缺文档推进
3. 未批准推进
4. 未经允许改 `Next`
5. 无法解释当前状态

第一版每次阻断都必须给出稳定 reason code。

## 12. Non-Functional Requirements

### 12.1 Local-first

第一版必须本地可运行，不依赖远端服务才能工作。

### 12.2 Traceable

每次批准和阻断都必须可追溯。

### 12.3 Recoverable

冷启动时必须能快速恢复当前状态。

### 12.4 Small Surface Area

第一版不追求大而全，优先确保最小闭环可靠。

## 13. Acceptance Criteria

第一版至少满足下面这些验收条件：

1. 用户能创建并锁定当前阶段
2. 用户能看到当前状态
3. 用户在缺少文档时无法进入下一阶段
4. 用户在未批准时无法进入下一阶段
5. 合法路径可以从 `goal` 一路推进到 `bmad`
6. 所有 block 都有明确 reason code

## 14. Out of Scope

第一版明确不做：

- 多人协作权限系统
- Web UI
- Discord / Hermes / Cloud DROID 深接入
- SaaS 平台化
- 自动生成整套产品内容

## 15. Open Questions

1. 第一版批准动作是否需要保留固定口令语义
2. `Next` 是独立字段，还是直接绑定在 phase state 里
3. 文档契约是用最小字段检查，还是强 frontmatter 方案
