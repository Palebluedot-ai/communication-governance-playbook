# Conversation State Machine v1

Status: CANONICAL_DRAFT  
Stage: MRD_SUPPORT  
Depends on:

- `docs/development-workflow.md`
- `docs/task-breakdown.md`
- `docs/mrd.md`
- `docs/prd.md`
- `imported/non-drift-communication-protocol/01_protocol/boundary-confirmation-v1.md`

## 1. 这份文档的作用

这份文档把前面聊过的“不要漂移”从口头规则，
收成一个可以落到产品里的状态机语义。

它当前不决定命令行语法，
也不决定数据库 schema。

它只决定：

> **什么叫继续当前阶段，什么叫申请切阶段，什么情况必须 block。**

## 2. Core Objects

第一版状态机至少围绕下面这些对象工作：

1. `global_goal`
2. `current_phase`
3. `phase_status`
4. `current_doc`
5. `current_next`
6. `approval_record`
7. `block_reason`

## 3. Phase Set

第一版固定 phase 为：

1. `goal`
2. `clarification`
3. `task_breakdown`
4. `mrd`
5. `prd`
6. `bmad`

这 6 个阶段必须按顺序前进，
不允许跳级。

## 4. Phase Status Set

第一版至少支持下面 4 种状态：

1. `not_started`
2. `in_progress`
3. `approved`
4. `blocked`

## 5. Allowed Transition Types

第一版只允许下面几类状态动作：

1. `start_phase`
2. `continue_phase`
3. `request_approval`
4. `approve_phase`
5. `request_transition`
6. `perform_transition`
7. `block`

这里最重要的区分是：

- `continue_phase` 不会切阶段
- `request_transition` 只是申请，不是生效
- `perform_transition` 必须建立在前置条件已满足之上

## 6. What Counts as Continue Phase

下面这些动作，默认都算继续当前阶段，
不能自动触发 phase transition：

1. 用户补充背景
2. 用户追问细节
3. 用户要求重印当前文档
4. 用户要求解释当前主线
5. 用户临时冒出一个想法，但没有明确要求换阶段
6. AI 自己建议“下一步可以做 X”

这类输入最多只能：

1. 更新当前文档
2. 追加 note
3. 更新 to-do
4. 触发 block explanation

不能直接改 `current_phase`。

## 7. What Counts as Request Transition

下面这些动作，才算申请切阶段：

1. 当前阶段文档已存在
2. 当前阶段文档被明确提出“请审阅/请通过”
3. 用户明确表达希望进入下一个阶段
4. 系统能指出目标下一阶段是什么

注意：

`request_transition` 仍然不是切阶段本身。

它只表示：

> 当前阶段已经进入“是否可以批准”的判断点。

## 8. Preconditions for Perform Transition

从一个 phase 真正进入下一个 phase，
第一版至少要满足下面 4 个条件：

1. 当前 phase 文档存在
2. 当前 phase 文档达到最低契约
3. 当前 phase 被显式批准
4. 目标 phase 是当前 phase 的合法下一阶段

少一个都不应放行。

## 9. Blocking Rules

第一版至少要 block 下面这些情况：

1. 当前 phase 未批准，试图进入下一阶段
2. 必需文档缺失，试图继续推进
3. 普通澄清问题被误当成 phase transition
4. 未经显式批准，试图改写 `current_next`
5. 试图跳过 `MRD -> PRD` 直接进 `BMaD`
6. imported / archive 材料与当前 `docs/` 冲突，却没有先完成收口

## 10. Minimum Reason Codes

第一版最小 reason code 集合建议固定为：

1. `BLOCK_MISSING_DOC`
2. `BLOCK_DOC_NOT_APPROVED`
3. `BLOCK_ILLEGAL_TRANSITION`
4. `BLOCK_NEXT_MUTATION`
5. `BLOCK_SCOPE_VIOLATION`
6. `BLOCK_SOURCE_OF_TRUTH_CONFLICT`

## 11. Self-Dogfooding Rule

这个状态机不是只拿来约束“以后别的项目”。

当前 repo 开发自己时，也必须被同一个状态机解释：

1. 现在在哪个 phase
2. 为什么还没到下一个 phase
3. 哪份文档是当前 gate
4. 当前允许动作是什么

如果这套状态机不能解释当前 repo 自己的开发过程，
那它还不算成立。

## 12. Relationship to Old Lock Semantics

历史材料里有几条非常重要的 lock 语义，
现在要保留精神，但不强行提前锁死实现细节：

1. `Global` 不应被随手改
2. `Next` 不应被普通澄清带偏
3. `proposal`、`approval`、`execution` 必须拆开
4. 出现漂移时，系统应该更偏向 fail-closed

但第一版当前还没有最后定死的，是：

1. 是否继续保留固定中文口令
2. `Next` 是否建成独立对象
3. approval 事件最终落在什么命令语法上

这些留给 `PRD` 和实现阶段再钉死。

## 13. 当前推荐用法

在当前项目阶段，这份文档的作用是：

1. 给 `MRD` 提供状态语义支撑
2. 给 `PRD` 提供 transition requirement 上游
3. 给后面的 CLI / guard / storage 设计提供统一解释层
