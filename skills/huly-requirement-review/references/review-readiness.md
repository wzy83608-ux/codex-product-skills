# 需求评审就绪规则

## 输入映射

从最新 Requirement Analysis Record 读取并映射：Requirement ID、Title、Raw Requirement、Background、Core Problem、Target User、Scenario、User Goal、FACT、HYPOTHESIS、UNKNOWN、Impact、Information Gap、Huly References、Analysis Recommendation、R 版本和创建/更新时间。

旧记录没有 Raw Requirement 或 Background 时保留为空，不强改旧 schema，也不因这两个兼容字段为空阻断第一阶段。

## 就绪条件

必须同时满足：Requirement ID 和 R 记录存在；Core Problem、Target User、Scenario、User Goal 非空；FACT/HYPOTHESIS/UNKNOWN 和 Information Gap 为数组结构；R 版本格式及 ID 有效；检查目标是当前最新 R 版本。

Readiness 只判断能否进入 Review，不判断内容正确性。未就绪时逐项保存缺失字段、来源版本、为什么阻止 Reviewer 和返回 Requirement Analysis 的建议动作；Reviewer 不得代补字段。

## 独立记录与指纹

`RRRxxx` 保存 Requirement ID、R 版本、业务/机器结果、Missing Fields、Blocking Reasons、Upstream Version、Fingerprint、检查时间、Reviewer Version、Confirmation 兼容模式、Huly 能力和未来 Search Plan。

Fingerprint 至少包含 Requirement ID、R 版本，以及 Core Problem、Target User、Scenario、User Goal、FACT/HYPOTHESIS/UNKNOWN、Information Gap 的哈希。

读取旧 RRR 时必须用当前最新 R 重算指纹。版本或任何关键哈希不同，返回：

- 中文：`需求评审就绪状态已失效，需要重新检查`
- 内部：`REVIEW_READINESS_INVALIDATED`

旧 RRR 不删除、不覆盖。

## 状态机

本阶段只有：

`需求评审尚未开始 → 正在检查需求评审是否就绪 → 需求评审未就绪 / 需求评审已就绪`

就绪后停止。正式评审、人工确认、PM 最终确认和 Priority 流转均属于后续阶段。
