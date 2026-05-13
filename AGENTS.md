# AGENTS.md - Product Exploration Kit

This repository contains two related but separate parts:

1. `agents/product-exploration-agent/` — the full product exploration sub-agent definition.
2. `skills/` — OpenClaw-compatible skills that can be installed independently.

## How To Use

### As A Sub-Agent

Load the product exploration sub-agent files in this order:

1. `agents/product-exploration-agent/SOUL.md`
2. `agents/product-exploration-agent/IDENTITY.md`
3. `agents/product-exploration-agent/AGENTS.md`

The sub-agent keeps the original role, behavior rules, collaboration workflow, evidence rules, and output standards.

### As OpenClaw Skills

Install or copy these skill directories:

- `skills/product-exploration`
- `skills/competitor-web-crawler`
- `skills/report-generator`
- `skills/difference-panel`

`product-exploration` is the workflow entry skill. It performs intent judgment first, then coordinates the crawler, report generator, and difference panel skills.

## Boundary

- Keep sub-agent identity and behavior settings under `agents/product-exploration-agent/`.
- Keep installable skill modules under `skills/`.
- Do not store API keys, app IDs, app secrets, access tokens, cookies, or private credentials in this repository.
