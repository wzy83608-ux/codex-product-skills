# PM 最终设计审批

## 进入条件

只有所有 Blocking Human Confirmation 均已完成、没有 Decision Conflict、Selected Solution 仍有效、最新 Detailed Design 包含相关确认追溯，并且最新 Reviewer 为 `PASS` 或无阻塞问题的 `CORRECTED` 时，才能进入。

进入时状态设为 `AWAITING_PM_FINAL_DESIGN_APPROVAL`，并保持 `prdEntryEligible: false`。

用户验证默认是可选能力：`userValidationRequired=false` 或 `USER_VALIDATION_NOT_REQUESTED` 不阻塞本 Gate。只有 PM 明确设置 `userValidationRequired=true` 时，才要求当前验证状态为 `USER_VALIDATION_COMPLETED`。本流程不新增 Technical Feasibility Approval、Technical Sign-off 或同等独立技术硬 Gate。

## 最终产品设计卡

向 PM 展示精简的决策卡，而不是完整 Agent 历史。至少包含 Core Problem、PM Selected Solution、AI Recommendation 和 alignment、Short/Long-term Fit、Existing Capability、最终流程、关键规则/状态/异常、权限、UI IA、验收标准摘要、已解决的 Human Confirmation、剩余 Non-blocking 问题、Reviewer 结果、相对上一版本的变化和上游版本链。

## PM 决策

- `APPROVE`：保存 `DESIGN_APPROVED`；只有这一操作可以令 `prdEntryEligible: true`。
- `REQUEST_CHANGES`：必须填写 Change Area、Change Request 和 Reason；保存 `DESIGN_CHANGES_REQUESTED`，随后创建新 DD 并重新 Reviewer。
- `RESELECT_SOLUTION`：保存 `SOLUTION_REEVALUATION_REQUIRED`，返回 Solution Comparison。
- `DEFER`：保存 `DESIGN_DEFERRED`，保持 PRD 关闭。

保存不可变的 `FDAxxx`，其中包含准确的 DD、DR、HDC 版本，决策、原因或修改要求，决策人/时间，上游版本链，审批指纹，以及 PM 当时看到的 Final Product Design Card Snapshot。

## 审批有效性

审批只对其绑定的准确 Solution、Solution Decision、Detailed Design、Reviewer、Human Confirmation Gate 和最新 Owner Response 指纹有效。出现更新的 DD、DR、HDC、Blocking Item、Solution 或 Owner Response 时，原审批有效状态变为 `APPROVAL_INVALIDATED`；出现新的 FDA 时，旧审批为 `SUPERSEDED`。

不得删除旧审批。每次检查 PRD Gate 前都要重新计算审批有效性。只有最新有效 FDA 为 `APPROVE`、指纹仍为当前版本、最新 Reviewer 仍通过、Blocking Confirmation 仍全部完成且无冲突、Selected Solution 仍有效时，才允许打开 PRD。
