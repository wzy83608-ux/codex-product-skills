# PRD 草稿生成规则

## Gate

只允许使用最新且仍有效的 `PRD_READY` 记录生成。运行时必须重新计算当前上游版本链和 FDA 审批指纹；未就绪、指纹变化、新 Blocking HC、新 Reviewer 阻塞或旧 FDA 失效时输出“PRD 生成被阻止”。

## 转写边界

只允许整理、归纳、去重、统一表达、章节化、编号和建立交叉引用。不得新增功能、改变 Selected Solution、改变流程或规则含义、新增状态/权限/异常、创造成功指标、技术实现或工期。缺少信息时记录“信息不足”、缺失来源、Blocking 属性、Owner 和建议动作。

## 草稿与校验

生成 `PRDxxx.json` 和同版本 `PRDxxx.md`。必须包含 21 节、Section 与 Item 两级来源溯源，以及 `FR ↔ BR ↔ State/Exception ↔ AC` 映射。轻量校验只允许修正文档结构；发现业务内容问题必须返回上游。

草稿状态固定为：`PRD 草稿已生成`、`prdDraftGenerated=true`、`prdReviewed=false`、`prdApproved=false`、`developmentEntryEligible=false`，下一步只能是 PRD Reviewer。

上游任何版本或审批指纹变化后，旧草稿保留但有效状态变为“PRD 草稿已失效”，必须重新执行 Readiness 并生成新版本。
