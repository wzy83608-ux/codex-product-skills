# 需求评审人工确认规则

## 适用范围

只把最新 Requirement Review 中真正 `BLOCKING` 的 Finding 转为人工确认项。Non-blocking Finding 仅展示，不创建强制确认。AI 发现 Blocking 不代表 AI 可以自行解决 Blocking。

正确链路是：Reviewer Finding → Human Confirmation → Evidence → 新 Reviewed Snapshot → Reviewer Re-run。不得直接 PASS、创建 RC 或进入 Priority。

## 记录与回答

Gate 定义保存为追加式 `RHCxxx`；每位 Owner 的回答独立保存为 `RHCRxxx`，不得相互覆盖；解析结果保存为独立 Outcome。RHC 至少绑定 R、RR、RRS、RCR、Blocking Finding 及 Core Problem、Target User、Scenario、User Goal 哈希。

每个 Item 保存 Confirmation ID、Finding/Field、Question、Background、Current/Missing Evidence、Why Blocking、Required/Optional Owners、Allowed Responses、Evidence Requirement 和 Resolution Rule。

Evidence Type 支持 `USER_FEEDBACK`、`BUSINESS_FEEDBACK`、`USER_INTERVIEW`、`BEHAVIOR_LOG`、`ANALYTICS`、`TIMEOUT_LOG`、`SUPPORT_TICKET`、`PRODUCTION_INCIDENT`、`CONFIRMED_PRODUCT_BEHAVIOR`、`OTHER`。

回答支持 `CONFIRMED_WITH_EVIDENCE`、`NOT_CONFIRMED`、`PARTIALLY_CONFIRMED`、`CONFLICTING_EVIDENCE`、`NEED_MORE_EVIDENCE`。确认或部分确认必须提供 Evidence Type、Summary 和可追溯 Source；缺少 Source 不关闭 Blocking。

## Owner 与冲突

Owner 规则按证据类型决定，不把 PM、Business、Data 固定为全部必答。问题存在性至少由 PM 或 Business 之一确认；引用 Behavior Log、Analytics 或 Timeout Log 时，必须由 Data 独立确认数据证据。

不同 Owner 的互斥结论保存为 `DECISION_CONFLICT`，AI 不选择答案。确认加“仍需证据”通常保持 `PENDING_EVIDENCE`，不自动视为冲突。

## 路由与复用

- `CONFIRMED_WITH_EVIDENCE`：满足 Owner、Evidence、无冲突和指纹有效后，生成新 RRS 并 Reviewer Re-run。
- `NOT_CONFIRMED`：进入 `REQUIREMENT_ANALYSIS_REVISIT_REQUIRED`，不修改 Core Problem。
- `PARTIALLY_CONFIRMED`：进入 `REQUIREMENT_ANALYSIS_REVISIT_REQUIRED`，由 Requirement Analysis 生成新 R。
- `CONFLICTING_EVIDENCE`：进入 `DECISION_CONFLICT`。
- `NEED_MORE_EVIDENCE`：保持 `NEED_HUMAN_CONFIRMATION` 和 Blocking。

历史决定只有语义域为 Problem Evidence、问题语义完全相同、证据可追溯、未过期、未被推翻且 Fingerprint 仍有效时才可 `REUSED_HUMAN_DECISION`。Solution/Design Decision（例如采用四阶段展示）不能复用为用户问题存在的证据。

## Reviewer Re-run 与放行

人工确认后重新检查 Core Problem、FACT/HYPOTHESIS/UNKNOWN、Evidence Strength、Blocking Gap、字段一致性和 Traceability。可靠人工证据可以在新 RRS 中增加 FACT，但不得修改原 R。Evidence Strength 按直接性、可靠性、独立来源和覆盖度重新计算，不因有人确认就自动 HIGH。

只有最新结果为 PASS/CORRECTED、Blocking Finding 和 Blocking Gap 均为 0、所有 RHC 无 Pending/Conflict 且 Fingerprint 有效，才能 `eligibleForPMRequirementConfirmation=true`。此时下一步只是等待 PM 最终确认需求，仍不得创建 RC 或进入 Priority。

上游 R 或 Problem Definition 关键字段变化后，旧 RHC 保留但标记 `HUMAN_CONFIRMATION_INVALIDATED`，必须重新评审其适用性。
