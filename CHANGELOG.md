# Changelog

## Agent-facing changes

### 1.0.2 — 2026-09-23

- `FIX-260923-2` A refused Hub or MCP call now names its recovery: REST errors carry `hint{reason, recovery, example?, see?}`, and MCP tool errors add one channel-neutral recovery line, so an agent can correct its call instead of guessing.

### 1.0.1 — 2026-09-23

- `FIX-260923-1` Skill archive links in `skills-index.json` now point at the MCP host the platform actually announces (`mcp.telegafirst.ru` until the `.com` zone is live), so agents can download both skills from the index.

### 1.0.0 — 2026-09-20

- `NEW-260920-1` First public agent-kit release: the `telegafirst-admin` and `telegafirst-onboarding-site-in-a-minute` skills, host connection guides, and release-ready package structure.

## Identifier policy

Every agent-facing change receives a permanent `{NEW|FIX|BC}-{YYMMDD}-{N}` identifier. Do not reuse, renumber, or replace an identifier after release; agents and freshness notices may cite it directly.
