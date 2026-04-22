---
name: report-generator
description: Report generation sub-skill for competitor research. Generates structured Markdown analysis reports from cleaned data. Called by the main competitor-research skill.
parent: competitor-research
---

# Report Generator Sub-skill

This sub-skill provides report generation templates and formatting rules. It is called by the main `competitor-research` skill.

## Report Structure

### Market Analysis Report

```markdown
# {Product} Market Analysis Report

## Executive Summary
[Brief overview of market findings]

## Market Overview
- Market size and growth trends
- Key market drivers and challenges

## Major Players
| # | Product | Vendor | Description | Source |
|---|---------|--------|-------------|--------|

## Product Comparison
| Dimension | Product A | Product B | Product C |
|-----------|-----------|-----------|-----------|

## Market Trends
1. Trend 1 with evidence
2. Trend 2 with evidence

## Recommendations
- For enterprise users: ...
- For small teams: ...
- For individual users: ...

## References
[1] Title - URL
```

### Product Deep Research Report

```markdown
# {Product} Deep Research Report

## Executive Summary
[Brief product overview]

## Product Positioning
- What it is and what problem it solves
- Target users and use cases

## Core Features
1. Feature 1: description
2. Feature 2: description

## Technology & Architecture
[Technical details if available]

## Pricing
| Plan | Price | Features |
|------|-------|----------|

## User Reviews & Ratings
[Summary of user feedback]

## Strengths & Weaknesses
**Strengths:**
- ...

**Weaknesses:**
- ...

## References
[1] Title - URL
```

### Competitive Comparison Report

```markdown
# {Product} Competitive Analysis Report

## Executive Summary
[Brief competitive landscape overview]

## Product Overview
| Attribute | Product A | Product B | Product C |
|-----------|-----------|-----------|-----------|
| Vendor | | | |
| Website | | | |
| Pricing | | | |
| Target User | | | |

## Feature Comparison Matrix
| Feature | Product A | Product B | Product C |
|---------|-----------|-----------|-----------|

## Differentiation Analysis
- Product A: key differentiator
- Product B: key differentiator

## Pricing Comparison
[Detailed pricing breakdown]

## Recommendations
- Best for X: Product A because...
- Best for Y: Product B because...

## References
[1] Title - URL
```

## Formatting Rules

1. **Headings**: Use `#` for title, `##` for sections, `###` for subsections
2. **Tables**: Always include headers, use left-aligned columns
3. **Bold**: Use `**bold**` for important terms and product names
4. **Sources**: Cite as `[1]`, `[2]` in text, list full references at the end
5. **Honesty**: Write "information not found" rather than fabricating content

## Quality Guidelines

1. Every claim should reference a source
2. Do not fabricate product features or pricing
3. Mark uncertain information as `[unverified]`
4. Keep language objective and professional
5. Include both strengths and weaknesses
