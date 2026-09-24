# Changelog

## Agent-facing changes

### 1.0.9 — 2026-09-24

- No agent-facing change of its own. It carries `FIX-260924-2` to the platform: the 1.0.8 package was published, but its platform rollout was rolled back when the worker roles took longer to boot than the rollout allowed, so production kept 1.0.6.

### 1.0.8 — 2026-09-24

The package and its GitHub Release were published; the platform rollout was rolled back (worker boot exceeded the 120 s budget), so production kept 1.0.6 and the fix below went live with 1.0.9.

- No agent-facing change of its own. It carries `FIX-260924-2` to the platform: the 1.0.7 package was published, but its platform rollout stopped at image prefetch before any role was updated.

### 1.0.7 — 2026-09-24

The package and its GitHub Release were published; the platform rollout stopped at image prefetch (Server A ran out of its 600 s download budget), so production kept 1.0.6 and the fix below went live with 1.0.8.

- `FIX-260924-2` Advancing `SERVICE_ACCOUNT_INVITED` now also gives the platform's pooled service bots the right to manage topics in all three supergroups, and widens a pooled bot the service account had promoted with fewer rights. Operator topics can then be renamed and closed, instead of every such call failing with `CHAT_ADMIN_REQUIRED`.

### 1.0.6 — 2026-09-24

- `FIX-260924-1` Advancing `SERVICE_ACCOUNT_INVITED` no longer stops on the tenant's own bot when the owner promoted it himself: a bot that is already an admin with the needed rights is left as it is, and one that is short of rights makes the step return a manual-action result that names the rights to switch on, instead of a transient MTProto error.

### 1.0.5 — 2026-09-24

- `FIX-260923-3` Onboarding's service-account step now brings every bot into the tenant's supergroups even when the service account has never met that bot, so advancing `SERVICE_ACCOUNT_INVITED` completes instead of ending in a transient "peer not found" error.

### 1.0.4 — not released

The `v1.0.4` source tag exists, but the release stopped while building and signing images, before the manifest, the GitHub Release or this package was published; its change ships in 1.0.5.

### 1.0.3 — not released

The `v1.0.3` source tag exists, but the release stopped at the image scan before anything was published; its change ships in 1.0.5.

### 1.0.2 — 2026-09-23

- `FIX-260923-2` A refused Hub or MCP call now names its recovery: REST errors carry `hint{reason, recovery, example?, see?}`, and MCP tool errors add one channel-neutral recovery line, so an agent can correct its call instead of guessing.

### 1.0.1 — 2026-09-23

- `FIX-260923-1` Skill archive links in `skills-index.json` now point at the MCP host the platform actually announces (`mcp.telegafirst.ru` until the `.com` zone is live), so agents can download both skills from the index.

### 1.0.0 — 2026-09-20

- `NEW-260920-1` First public agent-kit release: the `telegafirst-admin` and `telegafirst-onboarding-site-in-a-minute` skills, host connection guides, and release-ready package structure.

## Identifier policy

Every agent-facing change receives a permanent `{NEW|FIX|BC}-{YYMMDD}-{N}` identifier. Do not reuse, renumber, or replace an identifier after release; agents and freshness notices may cite it directly.
