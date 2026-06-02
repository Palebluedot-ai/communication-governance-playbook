# Product Boundary v1

Status: CANONICAL_DRAFT  
Stage: MRD_SUPPORT  
Depends on:

- `docs/global-goal.md`
- `docs/requirements-clarification.md`
- `docs/mrd.md`
- `imported/non-drift-communication-protocol/01_protocol/boundary-confirmation-v1.md`

## 1. 这份文档的作用

这份文档不负责描述实现细节，
也不替代 `MRD` 或 `PRD`。

它只回答一个问题：

> **这个产品第一版到底是什么，不是什么。**

之所以单独写这份文档，
是因为历史聊天里已经出现过一个典型问题：

- 目标是防漂移控制
- 但讨论一深入，就很容易滑向“多做一点功能”
- 最后边界不清，主线就会失焦

所以这里要把产品边界钉死。

## 2. Canonical Product Definition

第一版产品不是“更会聊天的 AI 助手”，
也不是“帮你自动生成所有研发文档的系统”。

第一版产品的 canonical 定义是：

> 一个把 AI 协作中的阶段推进做成强约束 workflow 的防漂移控制系统。

它最核心的职责只有 3 个：

1. 保持主线不被悄悄改写
2. 保持阶段不被悄悄切换
3. 把推进过程沉淀成可检查、可追溯的文档和状态

## 3. Global Goal Boundary

第一版默认继承并服从当前项目的 `Global Goal`：

> 把 AI 协作里的“防漂移”做成一个强约束控制系统，
> 优先保证 `Goal` 不变、`阶段` 不乱切、`澄清` 不偷偷改 `Next`。

这意味着：

- 它首先是控制系统
- 然后才是 workflow 工具
- 最后才考虑平台集成或协作体验优化

## 4. In Scope

第一版明确在范围内的，是下面这些：

1. 单 repo
2. 单 owner
3. 本地优先
4. 文档先行
5. 明确 phase order
6. 明确 approval gate
7. 明确 blocking rule
8. 明确当前允许动作
9. 自举开发时也遵守同一套规则

更具体地说，第一版一定要能约束下面这条主线：

`Global Goal -> 需求澄清 -> 开发任务拆解 -> MRD -> PRD -> BMaD 开发`

## 5. Out of Scope

第一版明确不在范围内的，是下面这些：

1. 多人权限模型
2. Web UI
3. Discord / Hermes / Cloud DROID 深接入
4. 通用 SaaS
5. 自动生成整份 MRD / PRD
6. 帮用户自动决定产品方向
7. 以“更聪明回复”替代 workflow gate

## 6. Non-Negotiables

下面这些属于第一版不能退让的原则：

1. 没有显式批准，不切阶段
2. 没有文档沉淀，不算完成
3. 用户追问、补充背景、临时灵感，不等于允许改主线
4. 系统开发自己时，也必须守同一套规则
5. imported / archive 里的历史材料不能自动视为当前真相

## 7. First-Version Promise

第一版对用户真正承诺的，不是“全自动研发”，
而是这 4 件事：

1. 你始终知道当前处在哪个阶段
2. 非法推进会被明确阻断
3. 每个阶段为什么能过、为什么不能过，是可解释的
4. 关键决定不会只留在聊天里

## 8. Boundary Tension

这个项目后面一定会遇到一个张力：

- 一边想尽快把它做成可执行工具
- 一边又很容易被平台接入、智能化体验、多角色协作吸走

这份文档的作用，就是在这个张力里充当“收口器”。

只要一个新需求不能直接增强下面任一项，
就不应该进入第一版：

1. phase clarity
2. transition control
3. document gate
4. blocking explainability

## 9. Relationship to Historical Materials

历史材料里真正被吸收进当前产品边界的，有这些：

1. `Goal / In-Scope / Out-of-Scope / Next / Drift Check` 的治理意识
2. `Global` 不应被随手改写
3. `Next` 不应因澄清问题自动漂移
4. 要区分 `proposal`、`approval`、`execution`
5. 要有 `self-dogfooding`

但这些历史材料在当前 repo 的身份是：

- `imported/`：证据来源
- `archive/`：归档来源
- `docs/`：当前 canonical source of truth

## 10. 下一步怎么用这份文档

这份文档通过后，它的作用不是继续扩展讨论，
而是给后续 3 件事提供边界：

1. `MRD` 只能在这个边界内被批准
2. `PRD` 只能展开这个边界内的产品要求
3. 后面写代码时，任何超出这里的功能都要被判成 phase-2 或 out-of-scope
