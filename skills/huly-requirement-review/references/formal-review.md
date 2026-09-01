# 正式需求质量评审规则

## 前置与边界

正式 Review 必须读取最新 R、与之匹配且仍有效的 RRR，并重算 Requirement Review Fingerprint。不一致时停止并返回“需求评审就绪状态已失效，需要重新检查”。历史 RC 只读，不能让 Reviewer 跳过检查。

Reviewer 检查分析质量，不重做分析。不得创造或替换 Core Problem、Target User、Scenario、User Goal、事实、Root Cause、优先级、Solution、合并或关闭决定。

## 八个维度

检查 Core Problem 清晰度、Solution Bias、FACT/HYPOTHESIS/UNKNOWN 分类、Evidence Strength、Information Gap 阻塞性、Problem Definition 字段一致性、逻辑跳跃和来源追溯。

Solution Bias 输出 `NONE/LOW/MEDIUM/HIGH`。UNKNOWN 和 Information Gap 按是否会改变 Core Problem、Target User、Scenario、User Goal、Scope 或问题是否成立，分为 `BLOCKING/NON_BLOCKING`。

Evidence Strength 只表示问题定义的证据质量，不代表 Priority：

- `HIGH`：核心结论有多项可靠来源，关键 UNKNOWN 不改变问题成立。
- `MEDIUM`：Problem 基本有证据，但影响、频率或解释仍依赖 Hypothesis。
- `LOW`：核心 Problem 主要靠推测、缺少直接事实或存在关键冲突。

## Huly Evidence Verification

优先读取 R 中已有 Huly References，最多读取少量高相关全文。系统文档可以证明系统环节、规则或原型存在，但不能单独证明用户困难、用户规模、频率或根因。Huly 标题和 Issue 链接不能替代正文证据。

本阶段不得执行 Similar Requirement、Existing Capability、Historical Bug、Conflict Detection 或最终 Bug/Requirement 分类。

## Finding 与修正

Finding 保存 ID、Category、中文类别、Severity、Affected Field、Original Content、Problem、Evidence、Recommendation、Auto Correctable、Requires Human Confirmation 和 Source。Severity 只分 `BLOCKING/NON_BLOCKING`。

可以自动修正 FACT/HYPOTHESIS 标签、UNKNOWN Blocking 标签、术语、重复、非业务表达、已有 Source 引用和字段格式。修正不得改变业务含义，也不得覆盖 R；生成独立 `RRSxxx` Reviewed Snapshot，并在 `RRxxx` 内保存快照及修正记录。

涉及新问题定义、用户、场景、目标、事实、Root Cause 或业务取舍时只能形成 Finding。

## 结果与状态

- `PASS`：无 Blocking，且无自动修改。
- `CORRECTED`：只修正非业务问题，修正后无 Blocking。
- `NEED_HUMAN_CONFIRMATION`：仍有 Blocking Finding。

RR 保存 Evidence Summary、Solution Bias、Findings、Blocking/Non-blocking 数量、Auto Corrections、RRS、分组 Information Gaps、Source Traceability、Fingerprint、时间和 Reviewer Version。

`PASS/CORRECTED` 只表示可以继续最小 Huly Context Review；本阶段固定 `eligibleForPMRequirementConfirmation=false`，下一阶段为 `HULY_CONTEXT_REVIEW`。只有当前有效 RCR 完成且无阻塞后，才可设置 PM Final Confirmation 就绪。本阶段始终不创建 RC、不触发 Priority。

上游 R 版本或关键字段变化后，旧 RR 保留，但有效状态必须为“需求评审结果已失效，需要重新评审”。
