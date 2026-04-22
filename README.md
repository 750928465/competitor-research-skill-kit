# Competitor Research Skill Kit

An AI-powered competitive analysis skill that works with any agent platform. No code dependencies — pure prompt-driven strategy that tells your AI agent how to search, analyze, and generate professional competitor research reports.

## How It Works

```
User Query → Intent Recognition → Search Strategy → Web Search → Data Cleaning → Report Generation
```

The skill provides the **strategy** (what to search, how to analyze, output format). Your AI agent provides the **execution** (web search, LLM reasoning).

## Quick Start

### Option 1: Git Clone

```bash
git clone https://github.com/750928465/competitor-research-skill-kit.git
```

Then load the skill in your agent:
- **Claude Code**: Copy to `.claude/skills/competitor-research/`
- **OpenClaw**: Install via skill registry
- **Other agents**: Point your agent to the `SKILL.md` file

### Option 2: Direct Download

Download the repository and extract the `SKILL.md`, `prompts/`, and `config/` folders to your agent's skill directory.

## Usage

Simply ask your agent to analyze a product or market:

```
"AI email assistant market analysis"
"Superhuman email client deep research"
"Superhuman vs Front vs Missive comparison"
```

## Three Analysis Scenarios

| Scenario | Example Input | Output Focus |
|----------|--------------|--------------|
| Market Analysis | "AI email assistant market" | Market size, players, trends |
| Product Deep Dive | "Superhuman email client" | Features, pricing, reviews |
| Competitive Comparison | "Superhuman vs Front" | Feature matrix, pricing, pros/cons |

## Project Structure

```
competitor-research-skill-kit/
├── SKILL.md                        # Main entry skill (start here)
├── skills/
│   ├── search-engine/
│   │   └── SKILL.md               # Search strategy sub-skill
│   └── report-generator/
│       └── SKILL.md               # Report generation sub-skill
├── prompts/
│   ├── intent_parser.md           # Intent recognition rules
│   ├── search_strategy.md         # Search query templates
│   ├── data_cleaning.md           # Data structuring rules
│   └── report_template.md         # Report format templates
├── config/
│   └── sources.yaml               # Trusted sources & exclusions
└── examples/
    └── example_output.md          # Sample report output
```

## Requirements

Your AI agent needs:
- **WebSearch** capability (required) — for retrieving search results
- **WebFetch** capability (optional) — for detailed page content
- **File reading** capability — to load prompts and config files

No API keys needed in the skill itself — the agent uses its own search tools.

## Platform Compatibility

Works with any AI agent that supports:
- Skill/slash command loading
- Web search tools
- Markdown output

Tested compatible with:
- Claude Code
- OpenClaw
- Hermes
- Any agent supporting SKILL.md format

## License

MIT License
