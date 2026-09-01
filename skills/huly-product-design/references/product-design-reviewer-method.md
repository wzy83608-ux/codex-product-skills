# 产品设计审查方法

## 进入条件与不可变规则

读取当前 Detailed Design 记录、其准确的 Selected Solution 和上游版本链。Reviewer 只审查草稿；不得重新生成方案、切换 PM 选择、修改优先级、引入其他方案的机制或生成 PRD 内容。

检查 Problem Coverage、Solution Consistency、Scope Creep、Business Rules、State Machine、Exceptions、Permissions、Data & Logging、UI IA、Acceptance Criteria，以及每一条 Assumption/Open Question。既要检查表面完整性，也要核对来源标签。

## Finding 与修正

将问题分为：

- 可安全自动修正：证据标签或 ID 不一致、重复、格式和逻辑错误，或由已确认规则直接推导的验收标准；
- 必须人工决定：技术可行性、状态真相源和恢复语义、新业务规则、权限边界、产品取舍或范围扩大。

不得因为一条假设看起来合理就把它改成已确认规则。保留原始 Detailed Design，只在独立的 Final Reviewed Design Snapshot 中应用修正。

范围扩大时标记 `SCOPE_CREEP`，说明超出的内容，并建议删除或拆成后续需求。若一项防御性实现只是为了让 Selected Solution 可靠运行，且没有引入新的用户能力或生命周期，则不属于范围扩大。

## 高风险检查

状态与异常审查必须明确检查模糊的终态真相：API 响应可能失败但后台任务实际成功；客户端重连时可能拿到过期状态；客户端超时后后台可能仍在运行；模型响应成功后结果解析仍可能失败。没有证据时，不得自行选择对账或取消语义。

权限审查必须区分：查看源对象、发起分析、查看已有结果、重新执行。只有方案未新增权限模型时才能保留 `NO_SPECIAL_PERMISSION_RULE`；这一结论不等于现有权限映射和执行中复核行为已经得到证明。

日志审查需要验证其是否支持排障、恢复、性能分析、行为分析和审计。不得为了显得完整而收集原始 Prompt、Token 或敏感载荷。

验收标准必须能够追溯到规则或明确标记的假设。只有预期行为已经确认时才能自动补充缺失 AC。对仍然阻塞的行为保持未决，不能把产品决定藏进测试用例。

## 假设与问题分类

将每条 Assumption 和 Open Question 标记为 `BLOCKING` 或 `NON_BLOCKING`，并从 `PM`、`Backend`、`Frontend`、`Data`、`AI/ML`、`Business` 中指定一个或多个 Owner。

每个阻塞人工项必须记录：

- 问题和 Owner；
- 为什么会改变设计；
- 答案为 YES 时如何继续；
- 答案为 NO 时如何调整。

只要还存在阻塞项，唯一有效结果就是 `NEED_HUMAN_CONFIRMATION`；保存 `AWAITING_DESIGN_CONFIRMATION` 并保持 `prdEntryEligible: false`。若无阻塞项，未修改时使用 `PASS`，安全修正后使用 `CORRECTED`，保存 `PRODUCT_DESIGN_REVIEWED`，并在执行任何 PRD 前停止。
