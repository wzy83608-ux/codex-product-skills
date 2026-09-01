# Huly 上下文评审规则

## 职责与边界

围绕已经确认的 Core Problem 检查 Huly 历史上下文，不重新分析需求、不改变 Core Problem、不决定优先级。不得自动合并、关闭、删除或永久修改 Requirement 类型，也不得创建 Human Confirmation、RC 或触发 Priority。

## 检索计划与上限

Search Plan 只能来自 Requirement ID、Title、Core Problem、Target User、Scenario、User Goal、FACT、HYPOTHESIS、UNKNOWN 和 Blocking Information Gap。必须分别检索：Similar Requirement、Existing Capability、Historical Bug、Existing Product Rule、Conflict Detection。

先服务端定向搜索，再排序候选，最后读取少量高相关全文。默认每类候选不超过 10、每类全文不超过 5、全部全文不超过 15。保存 Query 数、Candidate 数、Full Read 数、执行时间和是否使用全量扫描。需要突破上限时必须记录原因；接口不支持精确搜索时采用明确的最小范围 fallback。

## 分类规则

Similar Requirement 只能输出 `NO_SIMILAR_REQUIREMENT`、`RELATED_REQUIREMENT`、`POSSIBLE_DUPLICATE`、`HIGH_CONFIDENCE_DUPLICATE_CANDIDATE`。至少比较 Problem、Target User、Scenario 和 User Goal，标题相似不构成重复。历史 `DUPLICATE_CONFIRMED` 只读映射为 `HIGH_CONFIDENCE_DUPLICATE_CANDIDATE`，不回写旧记录，也不代表 PM 已决定 Duplicate/Merge/Close。

Existing Capability 只能输出 `NO_EXISTING_CAPABILITY`、`PARTIAL_EXISTING_CAPABILITY`、`EXISTING_CAPABILITY`、`UNKNOWN`。后台字段、接口或原型存在，不等于目标用户已能使用或感知。

Historical Bug 只能输出 `NO_RELATED_BUG`、`RELATED_BUG`、`POSSIBLE_BUG_MATCH`、`BUG_BEHAVIOR_CONFIRMED`。Bug Candidate 只能输出 `YES`、`NO`、`UNKNOWN`；只有现行 Expected Behavior 明确且实际行为违背时才可判 `YES`。

Conflict 必须区分 `PRODUCT_RULE_CONFLICT`、`REQUIREMENT_CONFLICT`、`DESIGN_CONFLICT`、`USER_SCOPE_CONFLICT`、`STATE_BEHAVIOR_CONFLICT`、`NO_CONFLICT`，并核验 Version、Updated At、Status、Current/Deprecated/Superseded。历史文档不同不自动构成冲突。

综合分类只允许 `NEW_REQUIREMENT`、`RELATED_TO_EXISTING`、`POSSIBLE_DUPLICATE`、`EXISTING_CAPABILITY_GAP`、`BUG_CANDIDATE`、`CONFLICT_REQUIRES_REVIEW`、`UNKNOWN`。

## 证据与 Reviewer 结果

每条证据保存 Evidence ID、Source Type/ID、Title、Relevant Section、Evidence Summary、Supports/Contradicts、Related Field、Confidence、Current/Deprecated/Unknown、Retrieved At。

原型、设计和技术能力只能证明背景，不能替代用户反馈、行为日志或真实问题证据。Evidence Strength 只按证据直接性和质量升级，不按命中数量升级。

Context Finding 类别为 `SIMILAR_REQUIREMENT`、`EXISTING_CAPABILITY`、`HISTORICAL_BUG`、`PRODUCT_RULE`、`CONFLICT`、`BUG_CLASSIFICATION`、`EVIDENCE_UPDATE`，严重度为 `BLOCKING` 或 `NON_BLOCKING`。

仅当高可信 `POSSIBLE_DUPLICATE` / `HIGH_CONFIDENCE_DUPLICATE_CANDIDATE`，或高可信且当前有效的产品规则冲突需要 PM 决策时，输出 `DUPLICATE_OR_CONFLICT`。普通 RELATED 不阻塞。

## 版本记录

独立保存 `RCRxxx`，至少包含搜索计划、查询、候选、全文来源、五类结论、Bug Candidate、Context Classification、新证据、Evidence Strength 前后、Blocking Gap 前后、Finding、性能、时间和指纹。

不得修改旧 RR/RRS/R。如果新证据改变 Evidence Strength、Finding、Gap 或 Review Result，生成新的 RR/RRS 并保留完整链路。只有 Review Result 为 PASS/CORRECTED、Blocking Finding 为 0 且无 Duplicate/Conflict，才允许 `eligibleForPMRequirementConfirmation = true`。
