# MRD

Status: READY_FOR_REVIEW  
Stage: MRD  
Depends on:

- `docs/global-goal.md`
- `docs/requirements-clarification.md`
- `docs/task-breakdown.md`
- `docs/mrd-prd-relationship.md`

## 1. Product Name

暂定名：

`communication-governance-playbook`

一句话定义：

一个把 AI 协作里的“防漂移”做成强约束 workflow 的控制系统。

## 1.5 Approval Intent

这份 `MRD` 现在的目标，不再只是“把想法记下来”，
而是进入一个可以被审阅、可以被拍板的状态。

这意味着它至少要回答清楚 4 件事：

1. 这个产品到底解决什么问题
2. 第一版只做什么，不做什么
3. 为什么先这样做，而不是一开始做大
4. 批准之后，下一阶段应该如何进入 `PRD`

## 2. Problem Statement

当前 AI 协作里最真实的问题不是“不会生成内容”，而是：

- 对话一长，AI 很容易顺着局部问题越走越深
- 澄清问题经常被错误地升级成切阶段
- 用户和 AI 都会被临时灵感带偏
- 产出看起来很多，但主线、路线图和 full picture 丢了

最终导致的成本包括：

- 时间浪费
- 注意力被打散
- 项目主线漂移
- 生成大量局部但低价值的内容

## 3. Why Now

你现在已经不是在验证“这个问题存不存在”，而是在反复真实工作里撞到它。

而且这个问题会随着 AI 参与开发变深：

- 越依赖 AI，越需要强边界
- 越多阶段协作，越需要 workflow 控制
- 越想产品化，越不能继续靠临场记忆

所以现在不是“以后再说”的问题，而是这个项目本身成立的前提。

## 4. Target User

### First User

第一版核心用户：

- 你自己
- 和 AI 一起做产品 / 做研发编排的人
- 对项目主线和 full picture 敏感，但容易被长对话细节拖走的人

### Characteristics

这类用户通常：

- 有明确项目目标
- 会持续追问细节
- 经常冒出新的想法
- 希望 AI 更像架构师，而不只是顺着聊的执行器

## 4.5 First Operating Context

第一版不是先做“给所有人都能直接上手”的 SaaS。

第一运行场景更明确：

- 你自己在本地 repo 里和 AI 协作
- 先把单线程、单 owner、单 repo 的约束跑顺
- 后面再决定怎么扩到多 thread、跨 channel、跨设备

## 5. Core Value Proposition

这个产品第一版提供的核心价值不是“更会聊天”，而是：

> **让 AI 协作从“靠聊天惯性推进”变成“受状态机约束推进”。**

具体价值包括：

1. 当前目标不被悄悄改写
2. 当前阶段不被悄悄切换
3. 澄清、拆解、MRD、开发之间有明确门禁
4. 每一步都有文档沉淀，不再只存在聊天里

## 6. First Version Scope

### In Scope

第一版只做下面这些：

- 单 repo
- 单 owner
- 本地优先
- CLI workflow
- 强阶段控制
- 强文档门禁
- 审计当前状态与历史切换
- 开发本项目自己时也受同一套门禁约束

### Out of Scope

第一版不做：

- 多人协作权限
- Web UI
- Discord / Hermes / Cloud DROID 深接入
- SaaS 形态
- 自动生成整份 MRD / PRD 内容

## 6.5 Narrowest Wedge

第一版最小楔子，不是“做完全部协议系统”，
而是先把下面这条主线做成可执行：

> 用户在单个 repo 内，必须按 `Global Goal -> 需求澄清 -> 开发任务拆解 -> MRD -> PRD -> BMaD` 前进；
> 缺文档、乱切阶段、越过门禁时，系统能明确阻断并说明原因。

只要这条最小主线跑通，这个产品就已经具备了第一性验证价值。

## 7. Core Workflow

第一版 workflow 固定为：

`Global Goal -> 需求澄清 -> 开发任务拆解 -> MRD -> PRD -> BMaD 开发`

系统需要保证：

- 未完成当前阶段，不能进入下一阶段
- 没有通过文档，不能切阶段
- 没有显式批准，不能偷偷改 `Next`
- 当前项目在开发自己时，也不能绕过这套规则

## 8. Product Thesis

这个产品的核心判断是：

> AI 协作里的漂移问题，本质不是 prompt 写得不够好，而是缺一个可执行的阶段控制器。

所以第一版不去追求“大而全”，而是先把下面这件事做透：

> 把阶段控制、文档门禁和 drift blocking 做成一个最小但强执行的 workflow engine。

## 8.5 Non-Negotiables

下面这些是第一版不能退让的原则：

1. 先有边界，再有扩展
2. 先有阶段门禁，再有自动化能力
3. 先把当前状态说清，再允许推进下一步
4. 先让系统能约束自己，再谈约束别的项目

## 9. Success Criteria

第一版成功至少满足：

1. 当前阶段可被系统明确识别
2. 非法切阶段会被阻断
3. 缺少必需文档时不能继续推进
4. 用户能看到当前状态、当前阻断原因、下一步允许动作
5. 本地文档和状态能支持冷启动接力
6. 这个系统在开发自己时，也能按同样的阶段门禁运行，不靠人工记忆维持主线

## 9.5 Approval Gate

这份 `MRD` 通过之前，不应该发生下面这些动作：

1. 把 `PRD pre-draft` 当成正式 `PRD`
2. 进入 `BMaD` 开发
3. 开始实现 runtime / state / command / guard 代码
4. 提前做平台接入和多端扩展

## 10. Risks

### Risk 1

把问题做成“文档模板系统”，没有真正的控制力。

### Risk 2

过早接入平台层，导致边界没锁死就开始扩展。

### Risk 3

把实现写得太重，反而延迟第一版验证。

### Risk 4

产品口头上承诺“防漂移”，
但开发它自己的过程仍然靠人工记忆兜底，导致产品承诺和开发现实脱节。

## 11. Dependencies

第一版依赖：

- 已确认的 `Global Goal`
- 已确认的需求澄清
- 已确认的任务拆解
- 已锁定的技术方向 `B`

## 12. Open Questions

1. 第一版是否需要 `PRD` 通过后才能允许 `bmad start`
2. 第一版的“批准”是纯手动命令，还是要保留固定口令语义
3. 第一版的文档契约字段，是轻量版还是直接承接旧协议里的固定字段
4. `Next` 是否应该作为独立对象建模，而不只是 phase state 的一个属性

## 13. Self-Dogfooding Requirement

这个项目有一个额外要求：

> 它在开发自己时，也必须能做到不漂移。

这不是锦上添花，而是核心验证方式。

如果这个系统在开发自己时还要靠人手工兜底，
说明它还没有真的解决问题。

第一版至少要做到：

1. 自己的开发流程也走 `Global Goal -> 需求澄清 -> 开发任务拆解 -> MRD -> PRD -> BMaD`
2. 自己的文档也必须先落盘再推进
3. 自己的阶段切换也必须可解释、可追溯

## 14. Next Suggested Step

当前建议下一步：

- 先把这份 `MRD` 修到可批准
- 再把现有 `PRD pre-draft` 转成正式 `PRD`
- 但先不要进入开发
- 先把 `workflow / state / command / guard` 的产品要求写清

## 15. Proposed Approval Statement

如果这份 `MRD` 通过，建议批准语句固定为：

> 同意本项目按这份 `MRD` 进入 `PRD` 阶段；
> 当前第一版目标锁定为“单 repo / 单 owner / 本地优先 / 强阶段控制”的防漂移 workflow engine；
> 未经新的显式批准，不扩展到平台接入、多角色权限或 SaaS 形态。
