---
name: search-engine
description: Advanced search strategy sub-skill for competitor research. Generates optimized multi-dimensional search queries based on intent type. Called by the main competitor-research skill.
parent: competitor-research
---

# Search Engine Sub-skill

This sub-skill provides advanced search query generation strategies for competitor research. It is called by the main `competitor-research` skill.

## Search Query Templates

### Market Analysis Queries

When the intent is `market_analysis`, search for:

```
1. "{product} market analysis report"
2. "{product} major vendors competitors"
3. "{product} industry trends 2024 2025"
4. "{product} market size revenue"
5. "{product} product comparison"
6. "{product} best options recommendations"
7. "{product} 主要厂商"
8. "{product} 市场规模 行业趋势"
```

### Product Deep Research Queries

When the intent is `product_deep_research`, search for:

```
1. "{product} official website"
2. "{product} product overview introduction"
3. "{product} core features capabilities"
4. "{product} technology architecture"
5. "{product} pricing business model"
6. "{product} user reviews pros cons"
7. "{product} use cases case studies"
8. "{product} 官网 产品介绍 核心功能"
```

### Competitive Comparison Queries

When the intent is `product_competition`, search for:

```
1. "{product} what is introduction"
2. "{product} core features"
3. "{product} official website"
4. "{product} competitors alternatives"
5. "{product} similar products"
6. "{product} vs comparison"
7. "{product} 替代方案 竞品"
8. "{product} 对比评测"
```

## Search Guidelines

1. **Product first**: Always start with queries about the product itself before searching competitors
2. **Bilingual coverage**: Generate both English and Chinese queries
3. **Deduplicate**: Remove duplicate URLs from combined results
4. **Credible sources**: Prioritize results from trusted domains (see `config/sources.yaml`)
5. **Result limit**: 5-10 results per query is sufficient

## Source Filtering

Exclude results from these domains:
- Social media: twitter.com, facebook.com, reddit.com, linkedin.com
- Review aggregators: g2.com, capterra.com, trustpilot.com
- Search engines: google.com/search, bing.com/search
- Video platforms: youtube.com
