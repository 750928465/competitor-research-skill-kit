---
name: competitor-research
description: Product competitive analysis skill. Automatically performs intent recognition, multi-dimensional search, data cleaning, deep analysis, and generates structured competitor research reports. Trigger when user needs to understand competitive landscape, compare products, or research a market.
version: 1.0.0
author: zhoubin
tags: [research, competitor, analysis, market, product]
---

# Competitor Research Skill

Automated product competitive analysis. Given a natural language query, this skill performs intent recognition, generates search strategies, cleans and structures results, and produces a comprehensive Markdown report.

## When to Use

Trigger this skill when the user:
- Wants to understand a product's competitive landscape
- Asks to compare products or find alternatives
- Requests market analysis for a specific domain
- Mentions keywords like "competitor", "vs", "market", "alternatives"

## Required Agent Tools

- **WebSearch** (required) — for retrieving web search results
- **WebFetch** (optional) — for fetching detailed page content when needed

## Workflow

### Step 1: Intent Recognition

Read `prompts/intent_parser.md` to identify the user's intent. Parse the query into one of three scenarios:

| Scenario | Keywords | Focus |
|----------|----------|-------|
| Market Analysis | "market", "industry", "landscape" | Market size, major players, trends |
| Product Deep Dive | Single product name | Features, architecture, reviews |
| Competitive Comparison | "vs", "compare", "alternatives" | Feature/price comparison |

Extract the `target_product` and `intent_type` from the query.

**Important**: Strip suffix modifiers from the product name (e.g., "AI email assistant feature comparison" → target_product = "AI email assistant", not "AI email assistant feature").

### Step 2: Generate Search Strategy

Read `prompts/search_strategy.md` and generate 6-8 search queries based on the identified intent:

1. **Product understanding first** — Always start with queries about the product itself
2. **Then search competitors** — Follow with competitive/alternative searches
3. **Bilingual queries** — Generate both Chinese and English queries for broader coverage

### Step 3: Execute Search

Use the agent's built-in WebSearch tool to execute each query. Collect all results and deduplicate by URL.

### Step 4: Data Cleaning

Read `prompts/data_cleaning.md` to structure the raw search results into:
- Product list (name, vendor, website, description)
- Feature dimensions
- Market insights
- Technical details

Exclude results from: social media, review aggregators, search engine pages.

### Step 5: Generate Report

Read `prompts/report_template.md` and generate a structured Markdown report with:
- Executive summary
- Market landscape (product table)
- Feature comparison matrix
- Deep insights
- Recommendations
- References with source URLs

### Step 6: Output

Return the complete Markdown report to the user.

## Configuration

Read `config/sources.yaml` for trusted source whitelists and search intent templates. Use these to prioritize results from credible sources.

## Sub-skills

- `skills/search-engine/SKILL.md` — Advanced search strategy for specific domains
- `skills/report-generator/SKILL.md` — Enhanced report generation with deeper analysis

## Examples

```
Input:  "AI email assistant market analysis"
Output: Market analysis report with major players, trends, and recommendations

Input:  "Superhuman email client"
Output: Deep product research report covering features, pricing, and reviews

Input:  "Superhuman vs Front vs Missive"
Output: Competitive comparison report with feature matrix and pricing analysis
```

## Notes

- Evidence-first principle: only include products with verifiable sources
- Never fabricate information not found in search results
- Mark uncertain information as "unverified"
- Always cite source URLs in the references section
