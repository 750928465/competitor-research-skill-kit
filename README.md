# Product Exploration Skill Kit

A platform-agnostic product exploration kit for competitor research. It includes both a reusable sub-agent definition and OpenClaw-compatible skills. It helps product managers move from "manually finding information" to reviewing AI-collected evidence, structured analysis, and competitor difference panels.

一个面向产品经理的产品探索智能体套件，同时包含可复用的 sub-agent 设定和 OpenClaw 兼容 skills。它通过自动抓取竞品网页、生成结构化竞品分析报告、输出竞品差异面板，帮助产品经理缩短信息搜集周期，并把注意力放在判断与决策上。

[English](#english) | [中文](#中文)

---

## English

### Positioning

This kit solves long manual research cycles and incomplete information gathering during product planning, feature iteration, and competitor monitoring.

It contains two parts:

- `agents/product-exploration-agent/`: full sub-agent identity, behavior, workflow, and output rules.
- `skills/`: installable OpenClaw-compatible skills.

### How It Works

```text
User Query → product-exploration workflow (intent judgment) → competitor-web-crawler (web_search + web_fetch) → report-generator → difference-panel → Competitor Analysis Report
```

The skill provides the **strategy**: what to search, which pages to fetch, how to clean evidence, how to structure the report, and how to render the difference panel. Your AI agent provides the **execution**: OpenClaw tools, file reading, and reasoning.

### Workflow / 工作流

```text
用户问题 User Query
   |
   v
product-exploration
先做意图判断 Intent Judgment
   |
   +--> market_landscape
   |      产品立项与市场格局
   |      Target: 5 core competitors, 3 credible sources each
   |
   +--> feature_iteration
   |      产品功能设计与迭代
   |      Focus: implementation, feedback, pitfalls, best practices
   |
   +--> market_monitoring
   |      市场动态与风险预警
   |      Focus: releases, pricing changes, campaigns, risk signals
   |
   +--> product_competition
          明确竞品对比
          Focus: positioning, features, pricing, differentiation

意图路由完成后 After intent routing
   |
   v
competitor-web-crawler
调用 web_search + web_fetch
   |
   v
证据充分性检查 Evidence Check
   |
   +--> 证据不足 Not enough evidence
   |      最多补充检索2轮 Max 2 supplemental rounds
   |      若无新增高价值来源则停止 Stop if no new high-value sources
   |
   +--> 证据足够 Enough evidence
          进入报告生成 Continue

Evidence Target:
   - 开放式市场研究默认识别5个核心竞品
   - 5 core competitors for open-ended market research
   - 每个核心竞品至少3个可信来源
   - At least 3 credible sources per core competitor
   - 不足部分写入“信息缺口”
   - Missing coverage is reported as information gaps

   |
   v
report-generator
清洗证据 + 选择分场景模板
   |
   v
difference-panel
生成竞品差异面板
领先 / 持平 / 缺失 / 未知 + 来源引用
   |
   v
最终 Markdown 竞品分析报告
Final Markdown Competitor Analysis Report
```

### Quick Start

```bash
git clone https://github.com/750928465/competitor-research-skill-kit.git
```

For OpenClaw skills, copy the skill directories to your agent's skill directory:
- **OpenClaw**: Copy `skills/*` to `~/.openclaw/workspace/skills/`
- **Claude Code**: Copy `skills/*` to `.claude/skills/`
- **Other agents**: Point your agent to the project root or import the `skills/` directory

For sub-agent usage, load:

1. `agents/product-exploration-agent/SOUL.md`
2. `agents/product-exploration-agent/IDENTITY.md`
3. `agents/product-exploration-agent/AGENTS.md`

### OpenClaw Starter Prompt

After installing the skills, copy this prompt into OpenClaw:

```text
你现在可以使用 product-exploration 相关 skills。请按 product-exploration workflow 执行产品探索任务：

1. 先做意图判断，识别为以下类型之一：
   - market_landscape：产品立项与市场格局
   - feature_iteration：产品功能设计与迭代
   - market_monitoring：市场动态与风险预警
   - product_competition：明确竞品对比

2. 根据意图调用 competitor-web-crawler：
   - 使用 web_search 发现来源
   - 使用 web_fetch 抓取官网、功能页、定价页、帮助中心、更新日志、新闻稿、可信媒体等关键页面

3. 证据目标：
   - 市场格局或开放式竞品研究默认识别 5 个核心竞品
   - 每个核心竞品至少收集 3 个可信来源
   - 证据不足时最多补充检索 2 轮
   - 仍不足时停止检索，并在“信息缺口”中说明

4. 调用 report-generator 生成分场景报告。
5. 调用 difference-panel 输出竞品差异面板，使用“领先 / 持平 / 缺失 / 未知”并绑定来源引用。
6. 最终输出 Markdown 竞品分析报告，必须包含：
   - 执行摘要
   - 研究范围与信息缺口
   - 竞品差异面板
   - 场景化分析
   - 机会点、风险点、待验证问题
   - References

请不要编造信息；未在搜索或抓取结果中找到的信息，明确标注“未在搜索结果中找到”。

测试任务：帮我调研一下 AI 邮箱市场情况，并输出主要竞品差异面板。
```

### Usage

Simply ask your agent:

```text
"AI customer service market opportunities"
"How do competitors implement membership points?"
"Superhuman vs Front comparison"
"Monitor Notion pricing and release updates"
```

### Core Scenarios

| Scenario | Example Input | Output Focus |
|----------|---------------|--------------|
| Product planning | "AI customer service market opportunities" | Market structure, key players, gaps, opportunities |
| Feature design and iteration | "Membership points competitor implementation" | Implementation patterns, user feedback, pitfalls, best practices |
| Market monitoring and risk alert | "Monitor Notion pricing and release updates" | Releases, pricing changes, campaigns, negative signals |

### Three Core Skills

| Skill | Responsibility |
|-------|----------------|
| `competitor-web-crawler` | Automatically discovers competitor pages with `web_search` and fetches key pages with `web_fetch` |
| `report-generator` | Cleans evidence and generates structured Markdown competitor analysis reports |
| `difference-panel` | Outputs a competitor difference panel with status labels and citations |

`product-exploration` is the workflow orchestrator skill. It first performs intent judgment, then coordinates the three core skills.

### Project Structure

```text
competitor-research-skill-kit/
├── AGENTS.md                                    # Repository-level usage guide
├── README.md
├── agents/
│   └── product-exploration-agent/              # Full sub-agent definition
│       ├── AGENTS.md
│       ├── SOUL.md
│       └── IDENTITY.md
└── skills/                                     # Installable skills
    ├── product-exploration/                     # Workflow orchestrator skill
    │   ├── SKILL.md
    │   └── references/
    │       └── workflow.md
    ├── competitor-web-crawler/                  # Competitor page discovery and fetching
    │   ├── SKILL.md
    │   ├── references/
    │   │   ├── intent_parser.md
    │   │   └── search_strategy.md
    │   └── assets/
    │       └── sources.yaml
    ├── report-generator/                        # Structured report generation
    │   ├── SKILL.md
    │   └── references/
    │       ├── data_cleaning.md
    │       ├── report_template.md
    │       └── example_output.md
    └── difference-panel/                        # Competitor difference panel
        ├── SKILL.md
        └── references/
            └── panel_template.md
```

### Requirements

Your AI agent needs:
- **OpenClaw `web_search` tool** (required) — for discovering competitor pages and market sources
- **OpenClaw `web_fetch` tool** (required) — for fetching official pages, pricing pages, docs, changelogs, and evidence-rich sources
- **File reading** capability — to load skill references and assets

For non-OpenClaw agents, map `web_search` and `web_fetch` to the platform's equivalent web search and URL fetch tools.

### Output

The final output is a Markdown competitor analysis report, including:
- Executive summary
- Research scope and information gaps
- Competitor difference panel
- Market/product/feature/monitoring analysis
- Opportunities, risks, and validation questions
- References

For market landscape and open-ended competitor research, the default evidence target is **5 core competitors**, with at least **3 credible sources per competitor** whenever available. If that coverage cannot be reached, the report must explicitly list the remaining gaps.

### Scenario-Specific Output Structures

The workflow is unified, but the report structure changes by scenario:

| Intent | Scenario | Report Structure |
|--------|----------|------------------|
| `market_landscape` | Product planning | Executive summary → Research scope and gaps → Difference panel → Market overview → Major players and positioning → User needs and use cases → Market gaps/opportunities → Risks and entry barriers → Positioning suggestions → References |
| `feature_iteration` | Feature design and iteration | Executive summary → Research scope and gaps → Difference panel → Feature goal and user scenarios → Competitor implementation breakdown → Core flow comparison → User feedback and pitfalls → Best practices → MVP/function suggestions → References |
| `market_monitoring` | Market monitoring and risk alert | Executive summary → Monitoring scope and gaps → Difference panel → Recent activity summary → Product releases → Pricing/package changes → Campaigns/launches → Negative signals and risks → Risk level and response suggestions → References |
| `product_competition` | Supplemental product comparison | Executive summary → Research scope and gaps → Difference panel → Product overview → Feature comparison → Pricing comparison → Differentiation → Scenario-based recommendations → References |

The three core scenarios are product planning, feature design/iteration, and market monitoring/risk alert. `product_competition` is kept as a supplemental comparison intent.

### License

MIT License

---

## 中文

### 定位

产品探索智能体用于把产品经理从传统的“人找信息”模式中释放出来。智能体负责搜集、抓取、整理竞品信息，并输出分析结论、竞品差异面板和竞品分析报告；产品经理只需要辨别信息、验证关键假设并做决策。

本项目包含两部分：

- `agents/product-exploration-agent/`：完整 sub-agent 设定，保留身份、行为规则、协作流程和输出标准。
- `skills/`：OpenClaw 兼容的可安装 skills。

解决的问题：
- 人工竞品分析周期长
- 信息搜集不全
- 功能方案设计缺少行业参照
- 核心竞品动态感知滞后

### 工作原理

```text
用户查询 → product-exploration workflow（意图判断）→ competitor-web-crawler（web_search + web_fetch）→ report-generator → difference-panel → 竞品分析报告
```

技能包提供**策略**：搜什么、抓哪些页面、如何清洗证据、如何生成报告、如何输出差异面板。你的 AI 助手提供**执行能力**：OpenClaw 工具调用、文件读取和推理。

### Workflow / 工作流

```text
用户问题 User Query
   |
   v
product-exploration
先做意图判断 Intent Judgment
   |
   +--> market_landscape
   |      产品立项与市场格局
   |      Target: 5 core competitors, 3 credible sources each
   |
   +--> feature_iteration
   |      产品功能设计与迭代
   |      Focus: implementation, feedback, pitfalls, best practices
   |
   +--> market_monitoring
   |      市场动态与风险预警
   |      Focus: releases, pricing changes, campaigns, risk signals
   |
   +--> product_competition
          明确竞品对比
          Focus: positioning, features, pricing, differentiation

意图路由完成后 After intent routing
   |
   v
competitor-web-crawler
调用 web_search + web_fetch
   |
   v
证据充分性检查 Evidence Check
   |
   +--> 证据不足 Not enough evidence
   |      最多补充检索2轮 Max 2 supplemental rounds
   |      若无新增高价值来源则停止 Stop if no new high-value sources
   |
   +--> 证据足够 Enough evidence
          进入报告生成 Continue

Evidence Target:
   - 开放式市场研究默认识别5个核心竞品
   - 5 core competitors for open-ended market research
   - 每个核心竞品至少3个可信来源
   - At least 3 credible sources per core competitor
   - 不足部分写入“信息缺口”
   - Missing coverage is reported as information gaps

   |
   v
report-generator
清洗证据 + 选择分场景模板
   |
   v
difference-panel
生成竞品差异面板
领先 / 持平 / 缺失 / 未知 + 来源引用
   |
   v
最终 Markdown 竞品分析报告
Final Markdown Competitor Analysis Report
```

### 快速开始

```bash
git clone https://github.com/750928465/competitor-research-skill-kit.git
```

如作为 OpenClaw skills 使用，将技能目录复制到你智能体的技能目录下：
- **OpenClaw**：复制 `skills/*` 到 `~/.openclaw/workspace/skills/`
- **Claude Code**：复制 `skills/*` 到 `.claude/skills/`
- **其他智能体**：指向项目根目录或导入 `skills/` 目录即可

如作为 sub-agent 使用，按顺序加载：

1. `agents/product-exploration-agent/SOUL.md`
2. `agents/product-exploration-agent/IDENTITY.md`
3. `agents/product-exploration-agent/AGENTS.md`

### OpenClaw 快速启动提示词

安装 skills 后，可以把下面这段提示词复制到 OpenClaw 中：

```text
你现在可以使用 product-exploration 相关 skills。请按 product-exploration workflow 执行产品探索任务：

1. 先做意图判断，识别为以下类型之一：
   - market_landscape：产品立项与市场格局
   - feature_iteration：产品功能设计与迭代
   - market_monitoring：市场动态与风险预警
   - product_competition：明确竞品对比

2. 根据意图调用 competitor-web-crawler：
   - 使用 web_search 发现来源
   - 使用 web_fetch 抓取官网、功能页、定价页、帮助中心、更新日志、新闻稿、可信媒体等关键页面

3. 证据目标：
   - 市场格局或开放式竞品研究默认识别 5 个核心竞品
   - 每个核心竞品至少收集 3 个可信来源
   - 证据不足时最多补充检索 2 轮
   - 仍不足时停止检索，并在“信息缺口”中说明

4. 调用 report-generator 生成分场景报告。
5. 调用 difference-panel 输出竞品差异面板，使用“领先 / 持平 / 缺失 / 未知”并绑定来源引用。
6. 最终输出 Markdown 竞品分析报告，必须包含：
   - 执行摘要
   - 研究范围与信息缺口
   - 竞品差异面板
   - 场景化分析
   - 机会点、风险点、待验证问题
   - References

请不要编造信息；未在搜索或抓取结果中找到的信息，明确标注“未在搜索结果中找到”。

测试任务：帮我调研一下 AI 邮箱市场情况，并输出主要竞品差异面板。
```

### 使用方式

直接向你的智能体提问：

```text
"AI客服市场立项分析"
"主流 SaaS 的会员积分体系怎么做"
"Superhuman vs Front 功能对比"
"监控 Notion 最近价格和版本更新"
```

### 核心场景

| 场景 | 示例输入 | 输出重点 |
|------|----------|----------|
| 产品立项与规划阶段 | "AI客服市场立项分析" | 市场格局、主要玩家、市场空白点、潜在机会 |
| 产品功能设计与迭代阶段 | "会员积分体系竞品怎么做" | 实现方式、用户反馈、优劣势、已知坑点、最佳实践 |
| 市场动态与风险预警 | "监控 Notion 最近价格和版本更新" | 版本更新、价格调整、市场活动、负面舆情、风险等级 |

### 三个核心技能

| Skill | 职责 |
|-------|------|
| `competitor-web-crawler` | 自动发现竞品网页，使用 `web_search` 检索并用 `web_fetch` 抓取关键页面 |
| `report-generator` | 清洗网页证据并生成结构化 Markdown 竞品分析报告 |
| `difference-panel` | 输出带状态标签和来源引用的竞品差异面板 |

`product-exploration` 是 workflow 主编排技能，负责先做意图判断，再协调上述三个核心技能。

### 项目结构

```text
competitor-research-skill-kit/
├── AGENTS.md                                    # 仓库级使用说明
├── README.md
├── agents/
│   └── product-exploration-agent/              # 完整 sub-agent 设定
│       ├── AGENTS.md
│       ├── SOUL.md
│       └── IDENTITY.md
└── skills/                                     # 可安装 skills
    ├── product-exploration/                     # workflow 主技能（意图判断 + 编排调度）
    │   ├── SKILL.md
    │   └── references/ (workflow)
    ├── competitor-web-crawler/                  # 竞品网页自动发现与抓取
    │   ├── SKILL.md
    │   ├── references/ (intent_parser, search_strategy)
    │   └── assets/     (sources.yaml)
    ├── report-generator/                        # 结构化报告生成
    │   ├── SKILL.md
    │   └── references/ (data_cleaning, report_template, example_output)
    └── difference-panel/                        # 竞品差异面板
        ├── SKILL.md
        └── references/ (panel_template)
```

### 工具要求

- **OpenClaw `web_search`**：必需，用于发现竞品网页和市场来源
- **OpenClaw `web_fetch`**：必需，用于抓取官网、定价页、功能页、文档、更新日志和高价值证据页
- **文件读取能力**：必需，用于加载各 skill 的 references 和 assets

非 OpenClaw 平台可将 `web_search` 和 `web_fetch` 映射为等价的网页搜索与 URL 抓取工具。

### 输出物

最终输出为 Markdown 竞品分析报告，包含：
- 执行摘要
- 研究范围与信息缺口
- 竞品差异面板
- 市场/产品/功能/动态分析
- 机会点、风险点和待验证问题
- References

针对市场格局和开放式竞品研究，默认证据目标是 **5 个核心竞品**，且每个核心竞品尽量收集至少 **3 个可信来源**。如果公开信息无法达到该覆盖度，报告必须在信息缺口中明确标注。

### 分场景输出架构

workflow 是统一的，但不同场景的报告结构不同：

| Intent | 场景 | 报告结构 |
|--------|------|----------|
| `market_landscape` | 产品立项与规划 | 执行摘要 → 研究范围与信息缺口 → 竞品差异面板 → 市场概览 → 主要玩家与定位 → 用户需求与典型场景 → 市场空白点/机会点 → 风险与进入壁垒 → 产品定位建议 → References |
| `feature_iteration` | 产品功能设计与迭代 | 执行摘要 → 研究范围与信息缺口 → 竞品差异面板 → 功能目标与用户场景 → 竞品实现方式拆解 → 核心流程对比 → 用户反馈与已知坑点 → 最佳实践提炼 → MVP/功能方案建议 → References |
| `market_monitoring` | 市场动态与风险预警 | 执行摘要 → 监控范围与信息缺口 → 竞品差异面板 → 近期动态摘要 → 版本更新 → 价格与套餐变化 → 市场活动/发布动作 → 负面舆情与风险信号 → 风险等级与响应建议 → References |
| `product_competition` | 补充竞品对比场景 | 执行摘要 → 研究范围与信息缺口 → 竞品差异面板 → 产品概览 → 功能对比 → 定价对比 → 差异化分析 → 场景化建议 → References |

其中前三类是核心场景，`product_competition` 作为补充对比意图保留。

### 许可证

MIT License
