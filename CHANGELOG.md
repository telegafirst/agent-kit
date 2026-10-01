# Changelog

## Agent-facing changes

### 1.0.20 — 2026-10-02

- Руководства подключения используют точный путь `next_action.execution.continue_with`. Безопасное возобновление принятого товара и уже одобренной первой оплаты выполняется агентом по явному инструменту; чтение состояния остаётся без побочных действий.

### 1.0.19 — 2026-10-01

- Обновлены контракты и справочники единого онбординга: полный текущий шаг, проверки товара и доставки, платёжного подключения, анкета ИИ и явное завершение. Сохранены прямые credentials, медиасообщения и выбранный бизнес.
- В руководствах подключения указаны полные пути полей ответа. Контрактные примеры и локальные прогоны отделены от проверки реального хоста.

### 1.0.18 — 2026-10-01

- `NEW-261001-3` RU-руководство Google Spark исправлено: оно описывает подключение пользовательского MCP-сервера в веб-версии и ограничения доступности. Проверка подключения на реальном хосте не заявлена.

### 1.0.17 — 2026-10-01

- `NEW-260930-1` Agents follow executable onboarding instructions, accept direct bot/provider credentials through typed tools, preserve whole product and delivery messages, and stop setup guidance after explicit server completion. File guides explain host import, byte upload and recovery without claiming unavailable host capabilities.
- The first live payment uses the scoped preparation/confirmation/execution pair, an explicit PayPal environment and the actual product card. Platform setup, warmup, reminder and follow-up messages also appear in the platform user's Open Lines topic.
- `NEW-261001-1` Полные RU-руководства подключения для Antigravity, ChatGPT, Claude web/Desktop, Claude Code, Codex, Cursor и Gemini Spark теперь входят в версионируемый Kit как копии канонических руководств. Они описывают настройку, авторизацию, проверку tools и бизнес-операций; существующие EN-руководства сохранены. Статус проверки конкретного хоста указан в руководстве и не означает подтверждённый live-запуск.
- `NEW-261001-2` Справочники REST и MCP раскрывают фактические поля запросов и ответов, ошибки, scopes и заголовки, чтобы агент мог исправить вызов по контракту. Агенту, использующему отдельный `@telegafirst/hub-sdk`, доступен opt-in conditional JSON: ответ с телом и ETag, ответ 304 без тела и структурированная ошибка. SDK не включён в Kit.

### 1.0.16 — 2026-09-30

- Agents start with personal onboarding state and its next action, distinguish owner and operator businesses, and can explicitly create their own business without borrowing another tenant’s permissions.
- Site setup preserves the storefront root and `/bio`, and waits for domain admission before presenting DNS verification records.

### 1.0.15 — 2026-09-30

- Connection guides and generated discovery now use the canonical `https://mcp.telegafirst.com` origin. Public AI requests enter through the Contabo proxy; the Hub and business operations continue to run on Server A.

### 1.0.14 — 2026-09-27

- `FIX-260927-1` The admin skill now directs agents to the available `get_onboarding_state` tool and its next action when setup is incomplete or a bot is silent.

### 1.0.13 — 2026-09-25

- No agent-facing change. Operator cards in the payments and operations supergroups (payment cards, the manager-session card, order notifications, the order task card) now link a MAX or VK buyer to that buyer's own channel profile instead of a `tg://user` link that pointed at an unrelated Telegram account; Telegram buyers keep the same link as before.

### 1.0.12 — 2026-09-25

- No agent-facing change. The platform release builds every Node app from one shared Dockerfile (ADR-308, queue 2): one prebuild compiles all of them once, each app gets a dependency layer that stays the same while its dependencies do, the release builder keeps its working cache under disk pressure, and Server B clones only the release tag.

### 1.0.11 — 2026-09-25

- No agent-facing change. The platform release's live message in the admin supergroup becomes a phone-readable image with a remaining-time estimate from past releases, and the release runner no longer blocks its own clock during migrations and smoke checks.

### 1.0.10 — 2026-09-25

- No agent-facing change. The platform release itself gets faster (ADR-308, queue 1): Trivy scans no longer hold build slots and run three at a time, signatures and health checks are verified in parallel, the gateway image is prefetched on Server B for migrations, and the Hub image's Agent Kit pack is checked from its label instead of pulling the image.

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
