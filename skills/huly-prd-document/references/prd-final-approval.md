# PM PRD 最终审批规则

## Gate 职责

此 Gate 只让 PM 判断：当前经 Reviewer 通过的 PRD 是否可以成为技术设计、研发和测试协作的正式产品输入。不得在此重新分析需求、重排优先级、重新选方案、重做产品设计或代替 Reviewer。

## 进入条件

进入前必须重新验证：最新 PRD 存在且未失效；最新 Reviewer 审查当前 PRD，结果为 `PASS` 或无 Blocking 的 `CORRECTED`；Blocking Finding 为 0；没有待处理 PRDHC；来源溯源完整；上游版本链、Product Design FDA 与 Approval Fingerprint 仍有效；`prdReviewed=true` 且尚未有当前有效审批。

Gate 就绪只能输出“等待产品经理最终确认 PRD”，不得自动创建 `APPROVE`。

## 最终审批卡

审批卡用中文摘要展示：需求和版本基本信息；最终 Core Problem、Target User、User Goal；PM Selected Solution、AI Recommendation 与 Alignment；In/Out/Derived Scope；核心用户流程；FR、BR、State、Exception、Permission、AC、Data/Logging 摘要；Reviewer 结果；不阻塞审批的待跟进项；PRD 版本变化；当前审批目标及完整指纹。

审批卡不复制整份PRD正文，也不允许重新选择方案。

若当前需求适合定义成功标准，最终审批前应有来源明确、可验证的量化或定性标准。若资料未提供，AI不得编造目标值或时间窗口；正式流程可由PM明确记录暂不设定的原因，独立草稿则省略该内容并列为待确认。

## PM 操作

- `APPROVE`：保存独立 `PRDFAxxx`；`prdApproved=true`、`technicalDesignEntryEligible=true`、`developmentEntryEligible=false`。
- `REQUEST_CHANGES`：必须记录 Change Area、Change Request 和 Reason，可选 Priority/Evidence；保留旧 PRD，生成新 PRD 并重新 Reviewer。涉及产品行为的修改先判断是否需要返回 Product Design。
- `RETURN_TO_PRODUCT_DESIGN`：状态为“需要重新进行产品设计”，关闭全部下游入口，按完整产品设计批准链重新推进。
- `DEFER`：保留所有文档和原因，状态为“PRD 已暂缓”，关闭全部下游入口。

## 独立记录与指纹

`PRDFAxxx` 保存 Requirement ID、PRD、PRDR、PRDHC、Product Design FDA、PM 决定、修改请求或原因、决定人与时间、审批卡快照、上游版本链，以及 PRD 内容、Product Design、Reviewer 和总 Approval Fingerprint。

总指纹必须绑定 RC、PC、S、SD、DD、DR、全部 HDC/HCR、FDA、PRD、PRDR、PRDHC。记录只追加，不写回 PRD Draft。

## 失效与替代

批准后出现任何新的绑定版本、PRD 内容、Reviewer、Blocking Finding、PRDHC 或失效 Product Design FDA，原批准记录保留但有效状态变为 `APPROVAL_INVALIDATED`，同时关闭 PRD 批准、技术设计和开发入口。

新审批记录产生后，旧 `PRDFAxxx` 有效状态为 `SUPERSEDED`，最新且指纹完全匹配的 `APPROVE` 才为 `ACTIVE`。

## 下游入口

`technicalDesignEntryEligible=true` 必须同时满足：最新 Reviewer 无阻塞、无待处理 PRDHC、最新有效 PRDFA 为 APPROVE、总指纹完全一致、Product Design FDA 有效、PRD 仍是最新版本且审批未失效。

无论 PM 是否批准，本阶段 `developmentEntryEligible` 始终为 `false`，并且不启动 Technical Design Agent。
