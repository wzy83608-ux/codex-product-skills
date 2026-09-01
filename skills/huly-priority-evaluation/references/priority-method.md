# 优先级评估方法

每次使用本 Skill 都必须读取本参考文件。

## 1. 证据收集

对每条需求，在资料可用时收集：

- Requirement ID、原始文档标题、正文和修改时间；
- 关联 Issue 的 Status、Type、现有 Priority、Milestone、Due Date、Estimate、Assignee 和 Relation；
- Requirement Agent 提供的 Core Problem、User、Scenario、FACT、HYPOTHESIS、UNKNOWN、Impact、Information Gap、Huly References 和上游 Recommendation；
- 关联 PRD、原型、技术评估、评论、测试任务和发布证据；
- 当前明确有效的 Strategy、OKR 和 Roadmap；
- 同一模块、季度、研发资源或战略目标下可比较的需求。

证据规则：

- 标题只能证明资料存在及其主题；行为、价值和状态结论必须有正文或元数据支持。
- 规划中的目标状态不是当前生产行为。
- 已有 P 标签和利益相关方的“很急”只是输入，不是结论。
- Development Estimate 只有来自明确研发估算、技术审查或已确认历史估算时才有效。
- 保留来源日期与有效期，不得把过期战略当作当前 Strategic Alignment。
- 引用来源无法读取时，说明访问限制并降低 Evidence Strength。

## 2. 优先级准入检查

每条需求只能输出一个准入状态：

- `READY_FOR_PRIORITY`
- `PRIORITY_NOT_READY`

检查：

- Core Problem 足够清晰，能把问题与用户提出的 UI/方案区分开；
- 至少存在基本的决策相关 FACT；
- 关键 UNKNOWN 不会反转优先级判断；
- Impact Scope 和 Severity 足以支持当前决策；
- Bug/Merge/Duplicate 分流已经处理；
- 需求不是需要拆分的无关事项集合；
- 文档不是空白或无标题；
- 来源和当前 Lifecycle State 不存在实质矛盾。

若已验证的严重事故、合规风险或硬承诺已经足以确立优先级，缺少精确用户数量不一定阻塞。相反，即使方案文档很详细，只要缺少问题、受影响用户或业务原因，仍然可能未就绪。

对 `PRIORITY_NOT_READY` 必须说明：

1. 缺少什么；
2. 为什么可能改变优先级；
3. 谁或哪个来源可以补充。

## 3. 价值维度

每个维度只能为 `LOW`、`MEDIUM`、`HIGH` 或 `UNKNOWN`，并包含结论、依据、支持 FACT、剩余 HYPOTHESIS 和 Huly 来源。

### 用户价值（User Value）

考虑影响范围、发生频率、严重程度，以及核心任务是否被阻断。

### 业务价值（Business Value）

考虑有证据支持的收入、转化、留存、运营效率、成本、关键客户承诺和实际损失。不得把可能收益转写为事实。

### 战略匹配（Strategic Alignment）

只能使用当前有效的 Strategy、OKR 或 Roadmap 证据。措辞相似不足以证明匹配；必须解释需求如何推进某个具名目标，否则输出 `UNKNOWN`。

### 紧急程度（Urgency）

考虑硬 Deadline、当前且持续扩大的损失、业务中断和外部平台变化。“老板/客户说急”不足以判定 `HIGH`。重要性与紧急性必须分开。

### 风险降低价值（Risk Reduction）

考虑已验证的资金、数据、权限、合规、稳定性、客户流失或业务中断风险。不得夸大假设风险。

## 4. 交付侧

### 开发成本（Development Cost）

只能使用 `LOW`、`MEDIUM`、`HIGH` 或 `UNKNOWN`。不得只根据文档长度或表面简单程度推断成本。没有研发 Estimate 或已接受的可比历史估算时，使用 `UNKNOWN`。

### 技术风险（Technical Risk）

只能使用 `LOW`、`MEDIUM`、`HIGH` 或 `UNKNOWN`。考虑复杂度、历史兼容、数据迁移、性能、稳定性、第三方 API、不确定性和回滚。即使成本仍为 `UNKNOWN`，只要方案范围明确，也可以有证据地判断技术风险。

### 依赖（Dependencies）

只能使用 `BLOCKING`、`NON_BLOCKING` 或 `UNKNOWN`。明确写出前置项，并在表格中把前置项排在被依赖项之前。

## 5. 相对优先级

对需求池，优先在以下范围内比较：

1. 同一产品模块；
2. 同一季度/Milestone；
3. 争夺同一研发资源；
4. 属于同一战略目标。

不得给整个 Huly 工作区强行排序。使用相关候选集，通常为 3～15 项，并解释最接近竞争需求之间的两两顺序。

优先使用明确关系，例如：

`需求A > 需求B`

原因：已确认的阻塞依赖、价值或紧急性更强，即使存在明确的成本或风险权衡，仍应优先。

不得把“尚未就绪但潜在价值高”的需求强行排到较低位置。应将其排除在已承诺顺序之外，并说明重新进入比较所需的 Gate。

## 6. 最终优先级

存在公司正式定义时使用正式定义；否则使用以下临时语义，并明确说明是临时标准：

- `P0`：需要立即处理的严重事故、合规/安全/数据/资金风险，或具有不可接受近期损失的硬外部 Deadline。必须极少使用，并要求强证据。
- `P1`：因为价值、依赖或时间窗口明显强于其他需求，应在当前规划周期交付。
- `P2`：有价值且理解充分，但应排在更高价值事项或前置事项之后。
- `P3`：影响较低、紧急性弱的 Backlog，延期造成的已证明成本有限。
- `PRIORITY_NOT_READY`：现有事实不足以支持可靠 P 等级。这是准入结果，不是另一个低优先级档位。

现有优先级与推荐优先级不同时，同时展示：

- Existing：Huly/标题/PRD 中发现的原值；
- Recommended：有证据支持的结论；
- 调整原因。

## 7. 表格构建

第一个结果必须始终为：

| 文档名称 | 优先级 | 评估原因 |
|---|---|---|

对已就绪项目，原因需要包含：

- 主要价值或风险驱动；
- 决定性的依赖或 Trade-off；
- 为什么高于或低于最接近的替代项。

对未就绪项目，原因需要包含：

- 缺少的信息；
- 为什么影响优先级；
- 如何补齐。

不要在该单元格中逐维度展开，只在表格后保留会改变决策的详细分析。

P0 可以在强事故、合规、安全、资金、数据风险或硬外部 Deadline 证据充分时独立判断。P1/P2/P3 缺少有效 Comparator Set 时只保存为 `priorityConfidence=PROVISIONAL`，展示 `PROVISIONAL_P1/P2/P3`；正式 PM Final Priority 仍只能是 P0/P1/P2/P3。确认 P1/P2 前展示 Comparator Set、比较范围、版本和有效期。

## 8. 独立 Priority Reviewer

最终交付前审查每一行并自动修正：

1. 是否存在没有依据的 `HIGH`？
2. 是否编造用户规模、业务价值、收入、损失或 Deadline？
3. 是否编造开发成本或 Capacity？
4. 是否把重要性当成紧急性？
5. 是否因为利益相关方级别而自动提高优先级？
6. 是否使用过期或猜测的战略？
7. 是否夸大风险？
8. 是否机械依赖 Score，而不是证据与权衡？
9. 相对顺序是否通过直接比较解释？
10. Evidence Strength 是否与结论匹配？
11. 是否把标题中的“P0 功能”章节或 P 标签误认为最终项目优先级？
12. 是否把测试完成误认为生产发布？

Reviewer 追加保存独立 `PRxxx`，结果只允许 `PASS`、`CORRECTED` 或 `NEED_HUMAN_CONFIRMATION`。自动修正只限编号、术语、来源引用、格式等非业务判断，并以修正快照保存，绝不覆盖 P；HIGH/P0、用户规模、收入、损失、Deadline、成本、Capacity、战略有效期、风险、比较池、相对顺序、Evidence Strength 与“测试不等于生产”都必须检查。只有最新有效 PR 为 PASS 或无阻塞 CORRECTED，才能进入 PM Priority Decision。
