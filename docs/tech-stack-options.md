# Tech Stack Options

## 这份文档回答什么

这份文档回答的不是“什么语言最流行”，而是：

> **什么技术栈最适合把这个项目做成一个有边界控制、有直接 workflow、不会漂移的系统。**

## 先锁 3 个前提

### Premise 1

这个项目第一版的核心，不是跨平台接入，而是 **强控制阶段和边界**。

也就是说，第一版优先级应是：

- 锁 `Goal`
- 锁当前阶段
- 锁阶段切换
- 锁文档产物

不是先追求：

- Discord / Hermes / Cloud DROID 全接通
- UI 漂亮
- 多人协作权限

### Premise 2

这个项目最重要的不是“回复格式漂亮”，而是 **状态机可执行**。

真正要控制的是：

- 当前在哪个阶段
- 能不能切到下一个阶段
- 缺了什么文档不能往下走
- 用户有没有显式批准切换

### Premise 3

第一版必须本地优先、文档优先、命令行优先。

原因很简单：

- 本地优先，才最容易验证和回滚
- 文档优先，才不会又变成“只聊不落盘”
- 命令行优先，才最适合强控制 workflow

---

## 方案 A：最小可执行 Guard CLI

### Summary

先做一个本地命令行工具，只管 3 件事：

1. 初始化工作流
2. 检查当前阶段是否合法
3. 控制阶段切换

### Effort

S

### Risk

Low

### Tech Stack

- Python 3.12+
- `uv`
- Markdown 文档
- YAML frontmatter
- JSON 状态文件
- `typer`：CLI
- `pydantic`：状态和输入校验
- `pytest`：规则测试

### Pros

- 最快落地
- 非常适合先把“不能漂移”做实
- 和你偏好的 Python 工作流一致

### Cons

- 审计能力偏弱
- 状态历史会比较薄
- 后面接平台适配器时需要补结构

### Reuses

- 当前 repo 里的 `docs/*.md`
- 历史里已经确认过的 `Goal / Next / 解锁 / Drift Check` 规则

---

## 方案 B：状态机驱动的 Workflow Engine

### Summary

先做一个本地的 workflow engine，把

`Global Goal -> 需求澄清 -> 任务拆解 -> MRD -> BMaD 开发`

做成强约束状态机。每次切阶段都要经过：

- 当前文档是否存在
- 当前文档是否通过
- 下一阶段是否被显式批准

### Effort

M

### Risk

Medium

### Tech Stack

- Python 3.12+
- `uv`
- Markdown 文档
- YAML frontmatter
- JSON 配置
- SQLite 状态库
- `typer`：CLI
- `pydantic`：schema 和 transition 校验
- `sqlmodel` 或 `sqlite3`：状态历史 / 审计轨迹
- `pytest`：状态机测试

### Pros

- 边界控制更强
- 历史轨迹更完整
- 以后接 Hermes / Codex / Discord 更自然

### Cons

- 比方案 A 多一层状态存储复杂度
- 第一版比纯 JSON 稍重

### Reuses

- 当前 3 份工作文档
- 历史线程里对 `双层解锁`、`Next 锁定`、`澄清不等于切阶段` 的确认

---

## 方案 C：服务化编排平台

### Summary

直接把它做成一个服务，带 API、适配器、状态管理和未来 UI 的雏形。

### Effort

L

### Risk

High

### Tech Stack

- Python 3.12+
- `uv`
- FastAPI
- Postgres
- Markdown + YAML
- JSON 配置
- `pydantic`
- `pytest`
- 后续可接前端

### Pros

- 长期形态最完整
- 对外产品化路径更顺

### Cons

- 现在太早
- 你当前真正的痛点还没被最短路径解决
- 很容易重演“还没锁边界，就开始堆结构”

### Reuses

- 只有规则层能复用
- 实现层大部分都要重做

---

## Recommendation

推荐 **方案 B：状态机驱动的 Workflow Engine**。

原因只有一句：

> 你的核心痛点不是“少个脚本”，而是“没有一个强执行的阶段控制器”。方案 B 正好打在这个点上，而且还没有重到服务化。

---

## 推荐技术栈（当前建议）

如果按推荐方案走，我建议当前技术栈就锁成下面这套：

### Language

- Python 3.12+

### Package / Runtime

- `uv`

### Core Data

- Markdown：阶段文档
- YAML frontmatter：文档元信息
- JSON：workflow 配置
- SQLite：当前状态 + 阶段历史 + 审计轨迹

### Core Libraries

- `typer`：做直接 workflow CLI
- `pydantic`：做 schema、状态、命令输入校验
- `pytest`：做状态机和 guard 测试

### Why this stack

因为它刚好满足你现在最在意的 3 件事：

1. **边界控制**
   - `pydantic` + transition rules 可以把非法切阶段直接拦掉

2. **直接 workflow**
   - `typer` 很适合把流程做成明确命令，比如：
     - `clarify start`
     - `clarify approve`
     - `task-breakdown start`
     - `mrd create`
     - `bmad start`

3. **不能漂移**
   - SQLite + 状态机可以明确记录：
     - 当前在哪个阶段
     - 是谁批准的
     - 缺什么文档
     - 为什么不能进入下一阶段

---

## 第一版不建议上的东西

- FastAPI
- Web UI
- Postgres
- 多人权限系统
- 平台深度接入
- LLM 自动重写 workflow

这些都不是现在第一版的主战场。

---

## 第一版直接 workflow 应该长什么样

技术栈选定后，workflow 最好直接长成这样：

1. `goal lock`
   - 锁全局目标

2. `clarification start`
   - 开始需求澄清

3. `clarification approve`
   - 澄清通过，允许进入任务拆解

4. `task-breakdown start`
   - 创建任务拆解文档

5. `task-breakdown approve`
   - 任务拆解通过

6. `mrd start`
   - 创建 MRD

7. `mrd approve`
   - MRD 通过

8. `bmad start`
   - 允许进入开发

这个顺序必须被工具控制，不能靠记忆。
