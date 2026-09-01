# 人工设计确认门

## 目的与进入条件

任何 Product Design Reviewer 记录中只要包含人工确认项，都使用本门禁。该机制是通用的，以 Requirement ID 和准确的 `DRxxx` 版本作为键。不得代表 PM、Backend、Frontend、Data、AI/ML 或 Business 填写答案。

每个确认项保存 Confirmation ID、问题、重要性、相关设计区域与引用、Owner、阻塞分类、当前假设、可选项、各 Owner 的决定与原因、证据、确认人、确认时间和状态，并明确 Decision Owner、Required Approvers、Consulted Owners、Veto Domains、Escalation Owner 与各 Owner 的 Decision Domain。

## 多 Owner 决策

为每个指定 Owner 创建独立回答位。一个 Owner 的回答不能完成另一个 Owner 的回答。Decision Owner 负责产品决定；Required Approvers 只确认各自责任范围；Consulted Owners 的意见保留但不自动阻塞。PM 不代替 Data/Security 等专业 Owner 确认专业约束，技术 Owner 也不代替 PM 作产品取舍。

回答状态包括 `PENDING`、`CONFIRMED`、`REJECTED` 和 `NEED_MORE_INFO`。不同责任域的不同结论不是冲突；只有同一 Decision Domain 出现互斥结论，或 Veto Owner 明确拒绝时，聚合状态才为 `DECISION_CONFLICT`，并提交 Escalation Owner。AI 不得选择或合并答案。历史仅有 `owners` 的记录按“全部为 Required Approvers、首位为 Decision Owner、各 Owner 独立域”兼容读取。

Blocking Item 决定门禁是否通过。Non-blocking Item 保持可见和可追溯，但不阻止下一版详细设计。任何处于拒绝、需要更多信息、待处理或冲突状态的 Blocking Item 都会保持 PRD 关闭。

## 对设计的影响

每个选项必须声明一种影响：

- `NO_DESIGN_CHANGE`
- `REVISE_DETAILED_DESIGN`
- `INVALIDATES_SELECTED_SOLUTION`

所有 Blocking Confirmation 完成且无冲突后，绝不能修改原始 DD 或 DR。必须以 Reviewer 的 Final Reviewed Design Snapshot 为基础创建新的 `DDxxx`，并记录：

- 已关闭的 Assumption 和 Open Question；
- 修改的 Rule、Transition、Exception、Permission 和 Acceptance Criteria；
- 每项修改对应的 Confirmation ID；
- 追溯链 `HC → Design Change → Rule/State/Exception/Permission/AC`。

若任何已确认选项推翻 Selected Solution，设置 `SOLUTION_REEVALUATION_REQUIRED` 并返回 Solution Comparison，不能继续生成同一方案的详细设计。

生成新 Detailed Design Version 后，必须重新运行 Product Design Reviewer 并创建新的 `DRxxx`。Reviewer 通过只代表可以进入 PM Final Design Approval，不能直接打开 PRD。
