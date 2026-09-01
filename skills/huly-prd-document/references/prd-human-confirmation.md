# PRD 人工确认与 Reviewer Re-run 规则

## 目标与边界

本阶段只解决 PRD Reviewer 的 Blocking Finding。不得重新分析需求、重做产品设计、改变 Selected Solution、生成技术方案、替 PM 作决定、审批 PRD 或放行开发。

## 先分类，再处理

每个 Blocking Finding 先分类为：

- `需要新的人工决策`：完整上游链中没有能回答该问题的有效决定。
- `已有确认结果，需同步修正`：HDC/HCR 已经回答，但最新 PRD 没有正确体现。

分类必须同时检查 Finding 的来源引用、HDC/HCR 版本、全部 Owner 响应、冲突状态、Selected Solution 是否仍有效、最新设计链和 FDA 审批指纹。仅凭相似文字不得认定为同一决定。

## 已有决定复用

已有决定只有在以下条件全部满足时可直接复用：

- 所有必需 Owner 都分别提交了 `CONFIRMED` 响应；
- 多 Owner 的决定一致，无 `DECISION_CONFLICT`；
- HDC/HCR 未被更新版本取代；
- 决定对应当前 Selected Solution 和最新批准设计链；
- 决定已包含在当前 FDA 指纹中；
- Finding 与决定所影响的 Rule、State、Exception 或 AC 能建立明确追溯。

满足后不得再次询问 Owner，也不得新建重复 Confirmation Item。生成 `PRDHCxxx`，记录来源 Reviewer、Finding、HDC/HCR、Owner 决定、证据、确认人与时间，并将 `requiresNewHumanDecision` 设为 `false`。

## 新决定

没有有效决定时，`PRDHCxxx` 为每个独立问题建立待确认项，记录问题、必要性、Owner、可选项、影响章节与条目、当前证据、不确认风险和状态。系统不得模拟 Owner 回答。多 Owner 必须分别确认；缺 Owner 或答案冲突时保持阻塞。

## 回写 PRD

人工决定不能覆盖旧 PRD。必须从 Reviewer 指定的最新修正版生成新的 `PRDxxx`，只同步决定直接影响的内容，并建立：

`PRDHC → HDC/HCR → Design Change → Section → Rule / State / Exception / AC`

可以更新既有 Business Rule、状态说明、Exception、流程和 UI 行为；可以新增从已确认决定直接推导且可测试的 AC。不得创建未经上游批准的新功能、Business Rule 或 State。版本历史必须说明变化、原因、来源、操作人和时间。

## Source of Truth

涉及客户端状态恢复时，必须明确谁是 Source of Truth。若已确认以后台任务真实状态为准，客户端刷新或重新进入后应重新获取、按版本协调并映射到现有阶段或终态；不得把刷新前的本地 UI 状态当作最终状态，也不得重复创建任务。

## Reviewer Re-run

同步或新决定回写形成新 PRD 后，必须重新执行完整 PRD Reviewer 并生成新 `PRDRxxx`。只有 Re-run 为 `PASS`，或无 Blocking Finding 的 `CORRECTED`，才允许等待 PM PRD 最终审批。

本阶段和 Re-run 始终保持：

- `prdApproved=false`
- `developmentEntryEligible=false`

不得因人工确认已完成直接进入审批或开发。

## 重复保护

同一来源 Reviewer 已生成 `PRDHCxxx` 后再次触发时，返回原记录和已同步 PRD，不再生成新的 PRDHC、PRD 或 PRDR。旧 PRD、旧 Reviewer 和上游记录必须保持不可变。
