# Product Exploration Workflow

## Purpose

Define the workflow for the `product-exploration` skill. This skill is the main orchestrator and must always start with intent judgment before calling downstream skills.

## Workflow Overview

```text
User Query
  -> Step 1: Intent Judgment
  -> Step 2: Crawl Plan
  -> Step 3: Competitor Web Crawling
  -> Step 4: Evidence Cleaning and Report Draft
  -> Step 5: Difference Panel
  -> Step 6: Final Report Assembly
```

## Step 1: Intent Judgment

Parse the user query into one of four intent types:

| Intent Type | Scenario | Downstream Focus |
|-------------|----------|------------------|
| `market_landscape` | 产品立项与规划 | Market structure, major players, gaps, opportunities |
| `feature_iteration` | 产品功能设计与迭代 | Feature implementation, feedback, pitfalls, best practices |
| `product_competition` | 多产品竞品对比 | Positioning, features, pricing, differentiation |
| `market_monitoring` | 市场动态与风险预警 | Releases, pricing changes, campaigns, negative signals |

The intent judgment must output:

```json
{
  "intent_type": "market_landscape | feature_iteration | product_competition | market_monitoring",
  "target_market": "string | null",
  "target_product": "string | null",
  "competitors": ["string"],
  "feature_focus": "string | null",
  "monitoring_scope": ["release | pricing | campaign | risk"],
  "missing_inputs": ["string"]
}
```

## Step 2: Crawl Plan

Based on `intent_type`, build a crawl plan:

- `market_landscape`: discover major vendors, official pages, market reports, media coverage.
- `feature_iteration`: discover product docs, help center pages, feature pages, user feedback, known pitfalls.
- `product_competition`: discover each product's official website, feature page, pricing page, alternatives and comparison pages.
- `market_monitoring`: discover changelogs, release notes, pricing pages, blogs, newsrooms, status pages, and credible risk signals.

## Step 3: Competitor Web Crawling

Call `competitor-web-crawler`:

- Use `web_search` for source discovery.
- Use `web_fetch` for official pages and evidence-rich URLs.
- Deduplicate and label every result with source type and credibility.

## Step 4: Evidence Cleaning and Report Draft

Call `report-generator`:

- Clean and normalize crawled evidence.
- Preserve source URLs and verification status.
- Select the report template that matches `intent_type`.
- Reserve a `## 竞品差异面板` section for the panel.

## Step 5: Difference Panel

Call `difference-panel`:

- Generate a dimension-by-product table.
- Use status labels: `领先`, `持平`, `缺失`, `未知`.
- Every non-empty judgment must cite a source.

## Step 6: Final Report Assembly

Return a complete Markdown competitor analysis report with:

- Executive summary
- Research scope and missing inputs
- Competitor difference panel
- Scenario-specific analysis
- Opportunities, risks, and validation questions
- References

## Routing Rules

1. Always complete intent judgment first.
2. If intent is ambiguous, choose the most useful executable path and list ambiguity in `missing_inputs`.
3. If the user provides competitors, crawl each named competitor separately.
4. If the user asks for monitoring, prioritize recent official and credible sources.
5. Never fabricate missing product facts, pricing, releases, or risks.
