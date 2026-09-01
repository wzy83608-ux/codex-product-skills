# 详细产品设计方法

## 输入与证据

读取用户选定的方案、需求定义、约束和可用证据。若没有正式方案记录，明确写出当前采用的方案描述及其来源，不要求特定版本库或编号。

每个结论或规则必须属于以下一种可追溯类型：

- `FACT`：由用户输入或可靠资料直接支持；
- `CONFIRMED_RULE`：由责任人、已选方案或明确约束确认；
- `ASSUMPTION`：为了让草稿完整而提出、但尚未确认的规则；
- `OPEN_QUESTION`：尚未解决，不得当作已经决定。

若未确认答案可能显著改变流程、状态、权限、恢复机制、数据契约或验收结果，标记 `NEED_HUMAN_CONFIRMATION`，只暂停受影响部分；其余部分可以继续设计。

## 必须包含的部分

### 1. 用户流程

覆盖 Entry、User Action、System Response、Decision Point、Success Path、Failure Path 和 Exit。包含替代及失败分支，不能只有 Happy Path。

### 2. 业务规则

每条规则包含 Rule ID、Description、Trigger、Condition、Result、Exception、Source、Confidence，以及 `CONFIRMED_RULE`、`ASSUMPTION` 或 `OPEN_QUESTION` 标签。

### 3. 状态设计

定义全部状态、允许与禁止的转换、触发条件和异常。检查不可达状态、非预期死路、循环、冲突和重复。

### 4. 异常设计

每项包含 Trigger、User Impact、System Behavior、Recovery、User Message 和 Logging Requirement。不要夹带其他未选择方案的行为。

### 5. 权限设计

明确查看、操作、修改、继承、权限变化和无权限界面。没有特殊权限规则时写 `NO_SPECIAL_PERMISSION_RULE`，不得编造角色。

### 6. 数据与日志

只保留具有产品分析、排障、安全或审计价值的数据。避免记录敏感载荷或没有决策价值的重复埋点。

### 7. UI 信息架构

定义 Page Goal、Primary Action、Secondary Action、Information Hierarchy、Main Sections、Status Display，以及 Loading、Empty、Error、Disabled、Confirmation 和 Feedback。本阶段不要求高保真视觉设计。

### 8. 验收标准

使用可独立验证的 Given / When / Then，覆盖 Happy Path、Exception、State Transition、Permission 和 Boundary Case。避免无法验证的主观描述。

## 保存与交付

以当前环境支持的方式交付 Markdown、JSON、文档或项目记录。若用户要求版本化，保存版本标识、方案来源、假设、开放问题、确认状态和时间；没有持久化工具时直接交付文件，不阻止设计完成。不得自动进入 PRD 或声称已获正式审批。
