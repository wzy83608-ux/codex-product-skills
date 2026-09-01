# Product Design Agent 第一阶段方法

## 1. 优先级评估 → 产品设计交接快照

交接由本地版本化记录库按 Requirement ID 自动读取，必须包含以下业务字段；PM 不重复填写：

- Requirement ID
- Requirement Title
- Core Problem
- Target User
- Scenario
- User Goal
- FACT
- HYPOTHESIS
- UNKNOWN
- Impact
- Information Gap
- Related Huly References
- PM Requirement Decision
- Priority Recommendation
- PM Final Priority
- Strategic Context
- Constraints
- Known Dependencies
- Evidence Strength

同时保存以下控制信息：

- Requirement Analysis 版本或来源链接；
- Priority Analysis 版本或来源链接；
- PM 是否明确确认 Priority、确认人和确认时间；
- 交接时间和数据版本；
- 每条 FACT/HYPOTHESIS/UNKNOWN 的原标签及来源。

继承字段在 Product Design 页面默认只读。若 PM 要修正 Core Problem、用户或优先级，应返回对应上游阶段，形成新版本后重新进入，不在本阶段覆盖旧值。

字段值可以是明确的 `UNKNOWN`，但不得无声缺失。`PM Final Priority` 与“PM 已确认”是两个不同字段：页面上存在一个 P 值，不等于 PM 已经确认。

## 2. 设计准入检查（Design Readiness Check）

逐项检查：

1. Core Problem 是否描述用户受阻的问题，而不是按钮或页面方案；
2. Target User 是否能区分实际使用者；
3. User Goal 是否能说明用户想完成的结果；
4. 是否存在 Blocking Unknown；
5. 会决定核心流程的业务规则是否已确认；
6. 是否存在可能令目标行为不可实现、且当前无法绕过的外部限制；
7. PM 是否明确确认 Priority。

### Blocking Unknown 判断法

对每个 UNKNOWN 问：不同答案是否可能显著改变以下任一项？

- 采用哪一类产品能力；
- 核心用户流程或关键状态；
- 是否允许执行关键动作；
- 业务规则、权限或数据口径；
- 外部平台/API 是否可行；
- 不可逆、资金、数据、合规或安全处理；
- PM 已确认的交付范围。

若是，标为 `BLOCKING`；否则标为 `NON_BLOCKING`。按钮文案、非关键展示顺序等通常不阻塞。是否允许重试（Retry）、是否可跨账号操作、统计口径是否包含当天等可能直接改变方案，应根据当前场景判断。

### Readiness 结论

- 所有准入项满足：`READY_FOR_DESIGN`
- 任一准入项不满足：`DESIGN_NOT_READY`

若为 `DESIGN_NOT_READY`，每个阻塞项必须写明：

- 缺什么；
- 为什么会显著改变产品方案；
- 谁应该补充；
- 通过访谈、业务规则确认、现网验证、Huly 文档、API 验证或其他何种方式补充；
- 补齐后从 Priority 页面保存新快照，再点击【进入产品设计】。

不因一般性的细节 UNKNOWN 拒绝进入设计，也不因文档很长就自动判定已经就绪。

## 3. 现有能力检查（Existing Capability Check）

### 必查证据类别

1. 相同或类似的现有功能；
2. 历史 PRD；
3. 历史产品方案；
4. 明确废弃、否决或下线的方案；
5. 相关 Bug；
6. 产品规则和数据口径；
7. 可以复用的现有流程或入口；
8. 权限规则；
9. 技术限制、第三方平台或 API 限制。

对每一类输出：已找到 / 未找到 / 无法确认、证据摘要、Huly 来源、证据限制。未检索到不等于不存在；无法读取外链或无法确认是否上线时应明确降级证据强度。

### 唯一分类

最终只选一个：

- `REUSE`：已验证的现有能力基本满足用户目标，不需要改变关键产品行为；可能只需采用或配置现有能力。
- `EXTEND`：已有可复用能力和主流程，只缺明确的增量覆盖；核心语义保持不变。
- `MODIFY`：已有相关能力，但当前行为、规则、权限或流程必须发生实质变化才能解决问题。
- `NEW_BUILD`：在检索充分、关键来源可读的前提下，确认不存在可承载目标的相关基础能力，需要建立新的产品能力。
- `UNKNOWN`：证据不足、来源不可读、是否上线不明、资料相互矛盾，或需求上下文不足以可靠分类。

不得把“只搜不到结果”直接判为 `NEW_BUILD`。不得把历史 PRD、原型或测试完成当成现网能力。若证据同时支持多个分类，选择 `UNKNOWN` 并指出需要验证的分界事实。

## 4. 第一阶段固定输出

### A. 交接接收结果

用表格展示每个继承字段的值或摘要、来源阶段、来源版本、是否完整。不得要求 PM 重填。

### B. 设计准入检查

第一行单独输出 `READY_FOR_DESIGN` 或 `DESIGN_NOT_READY`，然后列出各检查项及 Blocking/Non-blocking UNKNOWN。

未就绪时追加阻塞项表：缺什么、为什么阻塞、谁补充、怎么补、重新入口。

### C. 现有能力检查

先列 Huly 搜索方向和扫描范围，再按九类证据输出检查表，最后单独输出一个分类：`REUSE`、`EXTEND`、`MODIFY`、`NEW_BUILD` 或 `UNKNOWN`，并说明决定性证据及证据限制。

### D. 阶段结果

说明本次是否可以进入后续方案生成，但立即停止。固定注明：`方案生成器（Solution Generator）：未执行`。
