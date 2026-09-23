## MCP connection prompt — TODO(190-12-15-07)

1. MCP URL: `https://mcp.telegafirst.com/api/v1/mcp`
2. Connect it as a connector; OAuth completes when you add it.
3. Full platform description: `https://mcp.telegafirst.com/api/v1/external/llms-full.txt`
4. Task: help me configure and operate my TelegaFirst AI front office.

# TelegaFirst Agent Kit

Пакет навыков для агента владельца TelegaFirst: настройка фронт-офиса в Telegram, запуск воронок, поддержка, партнёрская программа и быстрый запуск сайта.

## Что внутри

- `telegafirst-admin` — основной навигационный навык с операционными плейбуками.
- `telegafirst-onboarding-site-in-a-minute` — запуск небольшого бизнес-сайта через существующие site-инструменты.

Используйте URL выше как удалённый MCP-коннектор. Если клиент умеет передавать только HTTP-заголовки, используйте выданный ему API-ключ с префиксом `tgf_api_`; не вставляйте ключи в документы, промпты или репозитории.

Для пошаговой настройки своего клиента откройте соответствующий файл в `docs/connect/`.
