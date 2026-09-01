# Product Design Agent：方案生成器与 PM 决策门

## 1. 进入条件

只有同一份已确认交接数据得到 `READY_FOR_DESIGN` 后才能执行本方法，并必须保留其来源版本。`DESIGN_NOT_READY` 绝不能生成或保存方案组。

## 2. 生成 2～3 个方案

从 Core Problem、Target User、Scenario、User Goal、已确认约束、依赖、Existing Capability 结论和证据出发。不得把用户原始提出的方案直接当作最终方案。

每个方案必须包含：

- Solution ID
- Solution Name
- Core Idea
- Problem Coverage
- Target User
- Main User Flow
- Existing Capability Reuse
- Key Business Rules
- Advantages
- Disadvantages
- Key Risks
- Assumptions
- Dependencies
- Open Questions
- Reversibility
- Short-term Fit：`HIGH / MEDIUM / LOW / UNKNOWN`，并说明依据
- Long-term Fit：`HIGH / MEDIUM / LOW / UNKNOWN`，并说明依据

方案必须在解决机制上不同，例如交互模式、运营模式、责任边界、时间模式、自动化/控制模式或能力架构。仅改变 UI 位置、文案、样式或布局，不构成不同方案。

## 3. 方案差异检查

比较前，必须针对每一对方案说明：

- 解决机制有什么不同；
- 用户流程或责任分配有什么不同；
- 业务/技术边界有什么不同；
- 在什么条件下其中一个更合适。

若多个方案本质上是同一机制，只是表现不同，输出 `SOLUTION_SET_NOT_DIVERSE`，废弃该组方案并自动重新生成。只有最终结果为 `SOLUTION_SET_DIVERSE` 的方案组才能进入比较。保存重新生成次数和简短的差异依据。

## 4. 方案比较

至少按以下固定维度比较全部方案：

- Problem Coverage
- User Experience
- Business Rule Complexity
- Development Complexity
- Technical Risk
- Operational Risk
- Existing Capability Reuse
- External Dependency
- User Control
- Scalability
- Reversibility
- Short-term Fit
- Long-term Fit

每个单元格只能为 `LOW`、`MEDIUM`、`HIGH` 或 `UNKNOWN`，并附简短依据。不同维度的高低含义不同：Coverage、Experience、Reuse、Control、Scalability、Reversibility 中，`HIGH` 表示收益更多；复杂度、风险和依赖中，`HIGH` 表示负担更大。

不得编造交付时长、人员、Story Point、成本或产能。若无法从明确范围或技术证据判断结构复杂度，输出 `UNKNOWN`。Development Complexity 不是研发 Estimate。

## 5. AI 推荐

AI Recommendation 允许三种结果：证据充分且明显更优时使用 `RECOMMEND_<SOLUTION_ID>`；推荐依赖未确认条件时使用 `CONDITIONAL_RECOMMENDATION` 并写明条件；关键成本、约束、用户偏好或外部限制不足以区分时使用 `NO_RECOMMENDATION_NEEDS_PM` 并说明缺口。推荐单一方案时需要说明：

- 为什么它适合当前已确认问题和规划周期；
- 为什么当前不推荐其他每个方案；
- 最大 Trade-off；
- 哪些假设或证据变化会改变推荐；
- Evidence / Confidence；
- 当前最优是否也是长期最优；
- 若不同，短期方案、长期演进方案和触发演进的条件。

不得隐藏不确定性，也不得机械选择 HIGH 最多的方案。Existing Capability Reuse 更高只是一个权衡因素，不能自动成为推荐最小改造方案的理由。必须明确平衡当前问题覆盖、用户结果、约束、风险、可逆性和长期匹配。

## 6. 人工决策门

保存方案组和 AI 推荐后，输出 `AWAITING_PM_SOLUTION_DECISION` 并停止。PM 可以执行：

- `ADOPT_AI_RECOMMENDATION`
- `SELECT_OTHER_SOLUTION`
- `REGENERATE_SOLUTIONS`
- `RECOMPARE_WITH_CONSTRAINTS`
- `PAUSE_DESIGN`

只有 PM 明确操作才能调用 `record-solution-decision`。必须保存：

- AI Recommended Solution；
- 适用时的 PM Selected Solution；
- 两者是否一致；
- PM 修改原因；
- 决策时间和决策人；
- 当时完整的 Solution Comparison Snapshot。

若 PM Selected Solution 等于 AI Recommended Solution，保存 `alignment: ALIGNED`，不要求 Override Reason。

若两者不同，保存 `alignment: OVERRIDDEN`，并要求一个结构化 Override Reason：

- `USER_EXPERIENCE`
- `TECHNICAL_FEASIBILITY`
- `LOWER_COST`
- `LOWER_RISK`
- `BUSINESS_CONSTRAINT`
- `STRATEGIC_REASON`
- `AI_MISSED_INFORMATION`
- `OTHER`

`OTHER` 还必须填写文字说明；其他结构化原因可以选填说明。不得把“未填写原因”当作允许保存 Override。

选择其他方案必须说明原因。重新生成、重新比较和暂缓都继续阻止详细产品设计。采用或选择方案可以让后续详细设计阶段具备准入条件，但本方法不执行该阶段。
