# Codex 产品工作 Skill 集

一组可独立安装和使用的 Codex Skill，覆盖资料检索、需求分析、需求评审、优先级、产品设计、PRD、项目管理、交付、测试、发布、数据分析、会议决策、复盘及安全的 Huly 写入集成。

这些 Skill 不依赖固定盘符、本机工作目录或其他 Skill。Huly、连接器、API 和浏览器均为按需使用的可选资料源。

## Skill 清单

| 中文名称 | 安装目录 | 用途 |
|---|---|---|
| 连接检测与只读资料检索 | `skills/huly-connection-toolkit` | 检查资料源连接并安全读取事项、文档和证据 |
| 产品数据分析 | `skills/huly-data-analysis` | 定义指标口径并分析变化、证据限制和决策含义 |
| 用户反馈与需求分析 | `skills/huly-demand-analysis` | 通过角色推演识别核心需求、目的、事实、假设和未知项 |
| 会议纪要与决策 | `skills/huly-meeting-decisions` | 整理会议决策、分歧、待确认事项和行动项 |
| 需求质量与证据评审 | `skills/huly-requirement-review` | 从使用者视角评审需求质量、实用性、用户价值和证据 |
| 需求优先级评估与排序 | `skills/huly-priority-evaluation` | 进行 P0/P1/P2/P3 评估和相对排序 |
| 产品方案与详细设计 | `skills/huly-product-design` | 生成和比较方案，设计流程、规则、状态与验收标准 |
| PRD 需求文档生成与审查 | `skills/huly-prd-document` | 按可靠依据生成PRD，流程图直接显示在正文，原型提供本地地址 |
| 产品交付综合总览 | `skills/huly-product-delivery-management` | 独立汇总决策、计划、进度、质量、发布、数据和复盘 |
| 项目进度监控 | `skills/huly-progress-monitoring` | 根据交付物和最新证据识别偏差、阻塞和恢复动作 |
| 产品项目管理 | `skills/huly-project-management` | 管理目标、范围、里程碑、Owner、依赖、风险和变更 |
| 产品发布与运营准备 | `skills/huly-release-operations` | 检查灰度、兼容、监控、回退、沟通和值守准备 |
| 产品项目复盘评估 | `skills/huly-retrospective-evaluation` | 对照目标和实际结果形成可验证的改进项 |
| 产品测试质量评估 | `skills/huly-test-quality` | 检查验收覆盖、关键缺陷、未测范围和残余风险 |
| Huly 文档安全写入接口 | `skills/huly-write-api` | 构建安全的 Huly 文档追加写入服务 |

## 安装

在 Codex 中发送仓库中某个 Skill 的 GitHub 目录链接，并要求安装。例如：

```text
请安装这个 Skill：
https://github.com/wzy83608-ux/codex-product-skills/tree/main/skills/huly-demand-analysis
```

也可以使用 Codex 自带的安装脚本：

```powershell
install-skill-from-github.py `
  --repo wzy83608-ux/codex-product-skills `
  --path skills/huly-demand-analysis
```

一次安装多个 Skill 时，在 `--path` 后依次列出目录。

## 使用与安全

- 安装后的技术调用名以各目录 `SKILL.md` 中的 `name` 为准。
- 分析类 Skill 默认不会修改外部系统。
- `huly-write-api` 涉及外部写入；运行实时测试前必须核对目标并取得明确授权。
- 不要把密码、Token、Cookie、真实业务数据或私有文档提交到仓库。

## 许可证

MIT License，详见 [LICENSE](LICENSE)。
