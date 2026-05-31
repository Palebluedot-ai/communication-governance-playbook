# Mainline Status

## 1. 当前主线是什么

当前主线不是写代码，不是接平台，也不是做 UI。

当前主线是：

> **先把这个产品自己的 workflow 文档体系锁住，再进入实现。**

更具体说，当前在做的是：

1. 锁产品目标
2. 锁开发流程
3. 锁 MRD
4. 锁 PRD
5. 然后才进入 BMaD 开发

## 2. 这个项目现在开发到了什么阶段

按当前文档状态，项目当前官方阶段处于：

`MRD 修订 / 待批准`

补充说明：

- `PRD pre-draft` 已经存在
- 但还没有正式完成 `MRD -> PRD` 的阶段切换

还没有进入：

- BMaD 开发
- runtime 实现
- state / command / guard 的代码实现

## 3. 已经完成了什么

目前已经沉淀完成的内容：

1. `Global Goal`
   - 文件：`docs/global-goal.md`
2. `需求澄清`
   - 文件：`docs/requirements-clarification.md`
3. `开发步骤拆解`
   - 文件：`docs/development-step-breakdown.md`
4. `任务块拆解`
   - 文件：`docs/task-breakdown.md`
5. `技术栈与方案选择`
   - 文件：`docs/tech-stack-options.md`
6. `MRD / PRD 关系说明`
   - 文件：`docs/mrd-prd-relationship.md`
7. `MRD 第一版草稿`
   - 文件：`docs/mrd.md`
8. `PRD 第一版草稿`
   - 文件：`docs/prd.md`

## 4. 当前阶段的目标是什么

当前这轮规格编写阶段的目标是：

1. 把 workflow 状态机说清
2. 把 phase / transition 说清
3. 把 document contract 说清
4. 把 command contract 说清
5. 把 guard / blocking rule 说清
6. 给 BMaD 开发一个不漂移的实现边界
   
更准确地说，当前目标是：

1. 先把 `MRD` 修到可批准
2. 再用现有 `PRD pre-draft` 转入正式 `PRD`
3. 然后才进入 `BMaD` 开发

## 5. 当前明确还没做什么

下面这些还没有开始：

1. 正式批准 `MRD`
2. 正式进入 `PRD` 阶段
3. 正式批准 `PRD`
4. BMaD 开发执行文档
5. Python CLI / SQLite / JSON / pydantic 的具体实现
6. 自动化 guard 代码

## 6. 下一步最合理是什么

按当前主线，下一步最合理的是：

1. 继续补强并确认 `MRD`
2. 把 `PRD pre-draft` 转成正式 `PRD`
3. 再进入 BMaD 开发

## 7. 这个系统能不能在开发自己时也不漂移

可以，而且应该写成明确要求。

这个项目不能只要求“未来对别的项目不漂移”，
它在开发自己的时候，也必须：

1. 不偷偷换阶段
2. 不跳过文档门禁
3. 不因为临时讨论就改主线
4. 不在 `PRD` 未通过前进入实现

也就是说，这个项目必须 **self-dogfooding**：

> 一边开发自己，一边用自己的防漂移规则约束自己。
