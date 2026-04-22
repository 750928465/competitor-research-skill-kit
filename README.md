# 竞品研究技能包

一个通用的 AI 竞品分析技能包，适用于任何智能体平台。零代码依赖，纯提示词驱动，告诉你的 AI 助手如何搜索、分析并生成专业的竞品研究报告。

## 工作原理

```
用户查询 → 意图识别 → 搜索策略生成 → 网页检索 → 数据清洗 → 报告生成
```

技能包提供**策略**（搜什么、怎么分析、输出什么格式），你的 AI 助手提供**执行能力**（网页搜索、LLM 推理）。

## 快速开始

### 方式一：Git Clone

```bash
git clone https://github.com/750928465/competitor-research-skill-kit.git
```

然后在你的智能体中加载：
- **Claude Code**：复制到 `.claude/skills/competitor-research/`
- **OpenClaw**：通过技能市场安装
- **其他智能体**：将 `SKILL.md` 指定为技能入口文件

### 方式二：直接下载

下载仓库，将 `SKILL.md`、`prompts/` 和 `config/` 解压到你的智能体技能目录即可。

## 使用方式

直接向你的智能体提问：

```
"AI 邮件助手市场分析"
"Superhuman 邮件客户端深度研究"
"Superhuman vs Front vs Missive 功能对比"
```

## 三种分析场景

| 场景 | 输入示例 | 输出重点 |
|------|---------|---------|
| 市场分析 | "AI 邮件助手市场" | 市场规模、主要玩家、发展趋势 |
| 产品深度研究 | "Superhuman 邮件客户端" | 功能特性、定价策略、用户评价 |
| 产品竞争对比 | "Superhuman vs Front" | 功能对比矩阵、定价分析、优劣势 |

## 项目结构

```
competitor-research-skill-kit/
├── SKILL.md                        # 主入口技能（从这里开始）
├── skills/
│   ├── search-engine/
│   │   └── SKILL.md               # 搜索策略子技能
│   └── report-generator/
│       └── SKILL.md               # 报告生成子技能
├── prompts/
│   ├── intent_parser.md           # 意图识别规则
│   ├── search_strategy.md         # 搜索查询模板
│   ├── data_cleaning.md           # 数据结构化规则
│   └── report_template.md         # 报告格式模板
├── config/
│   └── sources.yaml               # 可信来源白名单与排除规则
└── examples/
    └── example_output.md          # 示例报告输出
```

## 环境要求

你的 AI 智能体需要具备：
- **WebSearch** 能力（必需）——用于检索网页搜索结果
- **WebFetch** 能力（可选）——用于获取网页详情内容
- **文件读取** 能力——用于加载提示词和配置文件

技能包本身不需要 API Key，智能体使用自带的搜索工具即可。

## 平台兼容性

适用于任何支持以下能力的 AI 智能体：
- 技能/斜杠命令加载
- 网页搜索工具
- Markdown 输出

已验证兼容：
- Claude Code
- OpenClaw
- Hermes
- 任何支持 SKILL.md 格式的智能体

## 许可证

MIT License
