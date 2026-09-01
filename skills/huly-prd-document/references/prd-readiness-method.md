# PRD 就绪检查方法

## 自动读取的上游记录

- Requirement：R、RC
- Priority：P、PC
- Solution：S、SD
- Detailed Design：最新 DD
- Product Design Reviewer：与最新 DD 对应的最新 DR
- Human Confirmation：相关 HDC 及每个 Owner 的最新 HCR
- PM Final Design Approval：最新 FDA

## 阻塞条件

以下任一情况都输出“PRD 未就绪”：需求或优先级未由 PM 确认；无有效 Selected Solution；缺少当前 DD 或 DR；Reviewer 不是无阻塞的 PASS/CORRECTED；Blocking HC 未完成、需要更多信息或存在决定冲突；人工结论推翻方案核心前提；存在会改变设计的未决问题；FDA 缺失、非 APPROVE、已被新版本取代或审批指纹不匹配。

每个阻塞项必须说明问题、影响、Owner 和处理入口。不得由 AI 代替 Owner 决策。

## 非阻塞条件

Reviewer 明确分类为 NON_BLOCKING、且不会改变当前设计主干的 Assumption / Open Question 可以保留为后续项，不阻止 PRD 整理。它们进入标准 PRD 第 20 节，不得悄悄转成确定规则。

## 状态与保存

业务状态：PRD 尚未开始 → 正在检查 PRD 是否就绪 → PRD 未就绪 / PRD 已就绪。

每次检查追加保存 `PRCxxx`，包含 Requirement ID、结果、阻塞项、非阻塞项、完整上游版本链、FDA 审批指纹、检查时间、来源溯源和 21 节结构定义。到达“PRD 已就绪”后停止，`prdGenerated` 必须为 `false`。
