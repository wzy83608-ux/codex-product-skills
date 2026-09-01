# PM 需求最终确认规则

## 职责与进入条件

本 Gate 只确认经过 Reviewer 校验的 Problem Definition 是否可成为 Priority Agent 的正式输入，不重新分析需求、评优先级或设计方案。

进入前必须同时满足：最新 R、RRR、RR、RRS、RCR 和适用的 RHC 都有效且版本链一致；RR 为 PASS/CORRECTED；Blocking Finding/Gap 均为 0；无 Pending Human Confirmation、Decision Conflict 或 Duplicate/Conflict Blocking；所有 Review、Context、Human 指纹有效；`eligibleForPMRequirementConfirmation=true`。

满足时状态为 `AWAITING_PM_REQUIREMENT_FINAL_CONFIRMATION`，但 Requirement 尚未确认。

## Final Confirmation Card

Card 展示版本链、Core Problem/Target User/Scenario/User Goal、FACT/HYPOTHESIS/UNKNOWN 数量、Evidence Strength 与来源、Blocking/Non-blocking Gap、Reviewer Finding/Auto Correction/Solution Bias/Logic Jump/Traceability、Huly Context 分类、Human Confirmation 证据，以及 Reviewer 修正前后内容。

## PM 决策

- `APPROVE`：生成独立 `RCxxx`，状态 ACTIVE，`requirementConfirmed=true`、`priorityEntryEligible=true`，但不触发 Priority Agent。
- `REQUEST_CHANGES`：必须记录 Change Area、Request、Reason、Owner 和时间；返回 Requirement Analysis 生成新 R，不改旧 RRS。
- `RETURN_TO_REVIEW`：Requirement 内容暂不改，返回 Reviewer 检查证据分类、Context、冲突或自动修正。
- `DEFER`：保留全部历史，关闭 Requirement 和 Priority Entry。

## RC、指纹与失效

RC 保存 R/RRR/RR/RRS/RCR/RHC 版本、PM 决定、确认后的 Problem Definition、Evidence Strength、Gap、确认人/时间/评论、完整版本链、Confirmation Card、Confirmed Requirement Snapshot 和 Requirement Confirmation Fingerprint。

Fingerprint 至少绑定 Requirement ID、R/RRR/RR/RRS/RCR/RHC、Core Problem/Target User/Scenario/User Goal 哈希、FACT/HYPOTHESIS/UNKNOWN 哈希、Evidence Strength、Blocking Gap 和 Review Result。

产生任一更新版本、关键字段或证据变化、新 Blocking/Pending/Conflict、Duplicate/Conflict Blocking 或指纹不一致时，旧 RC 保留但有效状态为 `CONFIRMATION_INVALIDATED`，并关闭 Priority Entry。重新完成全链路并 APPROVE 后生成新 RC；旧 RC 有效状态为 `SUPERSEDED`，新 RC 为 ACTIVE。

没有新指纹结构的历史 RC 保留为 `LEGACY_CONFIRMATION`，不自动删除或判无效；新流程 Priority Entry 优先使用最新符合规则的 ACTIVE RC。

## Priority Entry

Priority Entry 只有最新 APPROVE RC 为 ACTIVE、RC 指纹和 Review 仍有效、无 Blocking/Pending/Conflict 且没有更新版本时才打开。

Priority Agent 必须读取 RC 内的 Confirmed Requirement Snapshot，包括确认后的 Problem Definition、FACT/HYPOTHESIS/UNKNOWN、Impact、Evidence Strength、Information Gap 和 Huly Context Summary。不得直接读取后来出现但未经 PM 确认的新 R。
