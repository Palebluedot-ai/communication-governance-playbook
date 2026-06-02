# Task Breakdown

## 这份文档的角色

这份文档承接：

- 已确认的 `Global Goal`
- 已确认的开发流程
- 已批准的技术方向 `B`

它回答的问题是：

> **如果我们要做一个“防漂移控制系统”，按方案 B 往下开发，应该先拆成哪些任务块。**

## 当前锁定前提

### Global Goal

把 AI 协作里的“防漂移”做成一个强约束控制系统，优先保证：

- `Goal` 不变
- `阶段` 不乱切
- `澄清` 不偷偷改 `Next`

### Approved Technical Direction

已通过：

- `方案 B：状态机驱动的 Workflow Engine`

核心技术栈：

- Python 3.12+
- `uv`
- Markdown
- YAML frontmatter
- JSON
- SQLite
- `typer`
- `pydantic`
- `pytest`

## 任务拆解原则

任务拆解先遵守 4 条原则：

1. 先控制 workflow，再扩展平台
2. 先控制阶段切换，再优化回复格式
3. 先把文档变成强约束输入，再写自动化
4. 先做单人单库版本，再考虑多人多频道

## Workstream 1：Workflow Canonical Spec

### Why

没有 canonical spec，后面状态机就会绑定在模糊语义上。

### Deliverables

- 一份 workflow spec 文档
- 一份 phase 定义表
- 一份 phase transition 规则表

### Core Questions

- 系统里一共有哪几个阶段
- 每个阶段允许什么输入
- 每个阶段必须产出什么文档
- 哪些动作算“继续当前阶段”
- 哪些动作算“切阶段”

### Current Canonical Docs

当前已开始沉淀到：

- `docs/development-workflow.md`
- `docs/conversation-state-machine-v1.md`

## Workstream 2：Document Contract

### Why

你已经反复强调“聊了很多但没落盘”。这说明文档不是附属品，而是状态机的输入。

### Deliverables

- `clarification` 文档契约
- `task breakdown` 文档契约
- `MRD` 文档契约
- `PRD` 文档契约
- `BMaD handoff` 文档契约

### Core Questions

- 每种文档最少必须有哪些字段
- 什么叫“文档存在”
- 什么叫“文档通过”
- 文档变更是否会影响当前阶段锁

### Current Canonical Docs

当前已开始沉淀到：

- `docs/product-boundary-v1.md`
- `docs/source-of-truth-map.md`
- `docs/mrd.md`
- `docs/prd.md`

## Workstream 3：State Model

### Why

如果没有明确状态模型，工具只能“看起来像在控流程”，实际上还是靠聊天。

### Deliverables

- 状态字段定义
- transition schema
- 审计字段定义

### Minimum State

第一版至少要有：

- `global_goal`
- `current_phase`
- `current_doc`
- `current_next`
- `phase_status`
- `approved_by`
- `approved_at`
- `blocked_reason`

## Workstream 4：Storage Design

### Why

你要的是“强执行 + 可追溯”，所以状态不能只在 Markdown 里漂着。

### Deliverables

- SQLite 表结构
- JSON workflow 配置结构
- 文档路径约定

### Storage Split

- Markdown：阶段文档与说明
- JSON：workflow 配置与 phase 规则
- SQLite：当前状态、历史、批准记录、阻断记录

## Workstream 5：CLI Workflow Commands

### Why

“直接 workflow” 不能只是概念，必须是硬命令。

### Deliverables

- 命令列表
- 每个命令的输入输出定义
- 错误码 / block 行为

### First Command Set

- `goal lock`
- `clarification start`
- `clarification approve`
- `task-breakdown start`
- `task-breakdown approve`
- `mrd start`
- `mrd approve`
- `prd start`
- `prd approve`
- `bmad start`
- `status show`
- `transition explain`

## Workstream 6：Guard Rules

### Why

这是整个产品最核心的地方。没有 guard，就只是一个项目模板。

### Deliverables

- 非法切阶段规则
- 缺文档阻断规则
- 未批准阻断规则
- drift reason code 列表

### First Blocking Rules

第一版至少要阻断：

1. 当前阶段未批准，试图进入下一阶段
2. 当前阶段缺文档，试图继续推进
3. 澄清中的补充问题，被错误提升为 phase transition
4. 未明确批准，试图改写 `current_next`

## Workstream 7：Testing Strategy

### Why

这个项目如果不先测 transition rule，就很容易“看上去很严”，实际一跑就漏。

### Deliverables

- phase transition 测试
- block case 测试
- happy path 测试

### First Test Focus

- 合法流转是否放行
- 非法流转是否阻断
- 重复批准是否幂等
- 缺文档时是否明确报错

## Workstream 8：Phase 1 Scope Boundary

### In Scope

第一版要完成的是：

- 单 repo
- 单 owner
- 单 workflow
- 本地 CLI
- 强阶段控制
- 强文档门禁

### Out of Scope

第一版不做：

- Discord / Hermes / Cloud DROID 深接入
- 多人权限
- Web UI
- 自动生成完整 MRD 内容
- 通用 SaaS 形态

## 推荐开发顺序

按现在信息，我建议顺序固定成这样：

1. Workflow Canonical Spec
2. Document Contract
3. State Model
4. Storage Design
5. CLI Workflow Commands
6. Guard Rules
7. Testing Strategy
8. 把以上内容收口进正式 `PRD`
9. `PRD` 通过后进入 `BMaD`

## 这一阶段的完成标准

这份任务拆解阶段完成，不是看是否开始写代码，而是看下面是否齐全：

1. 任务块边界明确
2. 第一版范围明确
3. 技术方向锁定
4. 可以进入 MRD / PRD，而不会再回到“到底做什么”的空转
