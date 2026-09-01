# Priority 入口、版本绑定与失效规则

## Priority Entry

Priority 只读取最新 ACTIVE RC 的 Confirmed Requirement Snapshot。进入条件包括：PM Decision=APPROVE、RC ACTIVE、requirementConfirmed/priorityEntryEligible=true、RC Fingerprint 与当前上游链有效、Confirmed Snapshot 存在、Reviewer PASS/CORRECTED、Blocking Finding/Gap 为 0、Human Confirmation 无 Pending/Conflict、无 Duplicate/Conflict Blocking。

结果只允许 `PRIORITY_READY` 或 `PRIORITY_NOT_READY`。未就绪时说明失败条件、当前 RC、最新 R、确认版本、原因和返回阶段。未确认的新 R 永远不能作为正式 Priority 输入；若它使 RC 失效则关闭入口，否则仍只能读取 RC Snapshot。

Priority Readiness `PRRxxx` 保存 Requirement ID、Source RC、RC Status、Confirmed Snapshot Version、RC Fingerprint、结果、Missing Conditions、Blocking Reasons、检查时间和 Readiness Version。

## Priority Analysis Fingerprint

P 必须绑定 Requirement ID、Source RC、RC Fingerprint、Confirmed Snapshot hash，以及 Core Problem、Target User、Scenario、User Goal、FACT/HYPOTHESIS/UNKNOWN、Evidence Strength 和 Information Gap 的哈希/值。P 同时保存 Source Confirmed Snapshot 与完整上游版本链，确保可以追溯到 R/RR/RRS/RCR/RHC。

## Invalidation 与下游关闭

Source RC Invalidated/Superseded、RC 或 Snapshot 指纹变化、Problem Definition 或证据变化、Blocking/Pending/Conflict 重新出现，或 Priority Fingerprint 不匹配时，P 有效状态为 `PRIORITY_INVALIDATED`。对应 Priority Review、PC 分别为 `PRIORITY_REVIEW_INVALIDATED`、`PRIORITY_CONFIRMATION_INVALIDATED`，并设置 `productDesignEntryEligible=false`。

失效只关闭有效性，不删除历史，也不自动重新评估。新 ACTIVE RC 通过 Entry 后由用户重新运行 Priority，形成新 P/Review/PC；旧链有效状态为 `SUPERSEDED`，新链为 ACTIVE。

没有 Source RC Fingerprint 的历史 P 标记 `LEGACY_PRIORITY`，不补造 Fingerprint。Legacy P/PC 保留查看，但不能打开新流程 Product Design。

## 边界

本机制不修改 User Value、Business Value、Strategic Alignment、Urgency、Risk Reduction、Cost、Technical Risk、Dependency、Relative Comparison、Evidence Strength 或 P0/P1/P2/P3 算法。
