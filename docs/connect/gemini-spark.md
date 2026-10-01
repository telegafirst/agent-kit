---
slug: connect-gemini-spark
kind: page
audience: public
locale: ru
title: "Подключение Gemini Spark"
parent_slug: mcp-guide
sort_order: 66
---

Подключение даёт вашему агенту доступ к AI-фронт-офису в Telegram в пределах ваших прав. Нужны аккаунт выбранного клиента, Telegram для входа человека и доступный HTTPS endpoint. Настройка Spark сверена с официальными источниками 2026-10-01; успешный ручной проход этого хоста и его конкретная версия здесь не заявлены.

## Настройка

Google документирует подключение custom apps по MCP URL для Gemini Spark; эта возможность отмечена в [обновлении от 29 июня 2026](https://support.google.com/gemini/answer/17171264?hl=en). Для custom apps нужны возраст от 18 лет, нахождение в США, личный Google-аккаунт и включённый Keep Activity. Рабочие и учебные аккаунты не поддерживаются; функция доступна только на английском.

1. Откройте [веб-приложение Gemini](https://gemini.google.com).
2. Перейдите в Settings → Connected Apps; если пункта нет, откройте Personal Intelligence → Connected Apps.
3. В разделе Custom apps добавьте приложение с MCP URL `https://mcp.telegafirst.com/api/v1/mcp`.
4. Нажмите Next и следуйте экранным шагам авторизации. Для обращения к подключённому приложению в запросе введите `@` и выберите его.

Добавление custom app выполняется только в веб-приложении; после подключения им можно пользоваться в веб- и мобильном Gemini. Эти шаги не подтверждают custom MCP в приложении для Mac или успешное подключение TelegaFirst в вашей сессии. [Официальная инструкция Google](https://support.google.com/gemini/answer/17209137?co=GENIE.Platform%3DDesktop&hl=en).

### Отдельная альтернатива: Gemini CLI

Gemini CLI — другой продукт, это не подтверждение Spark. Его официальная документация описывает Streamable HTTP через `httpUrl` и OAuth:

```json
{"mcpServers":{"telegafirst":{"httpUrl":"https://mcp.telegafirst.com/api/v1/mcp","trust":false}}}
```

В Gemini CLI используйте `/mcp auth telegafirst` и браузерный вход. [Документация Gemini CLI](https://geminicli.com/docs/tools/mcp-server/). Если custom apps недоступны вашему аккаунту или выбранному интерфейсу Spark, используйте другой поддерживающий клиент или [независимый curl-сценарий](/docs/curl-device-grant).

## Discovery и согласие

Следующие шаги описывают протокол TelegaFirst после добавления custom app в поддерживаемом веб-интерфейсе Spark или настройки отдельного Gemini CLI. Документированный путь Spark не подтверждает завершённый OAuth или успешный вызов в вашей сессии.

Используйте canonical endpoint `https://mcp.telegafirst.com/api/v1/mcp` с Streamable HTTP. При первом неавторизованном запросе сервер возвращает HTTP 401 с `WWW-Authenticate`; клиент получает OAuth discovery и открывает вход. Подтвердите показанное согласие в Telegram от своего имени. Отказ завершает вход, а не создаёт обходной API key. Подробности и различия credentials — [авторизация](/docs/authorization).

## Инструменты и бизнес

После OAuth обновите `tools/list`. У нового владельца без бизнеса доступны три dedicated-инструмента: `get_onboarding_state`, `check_slug_availability` и `claim_slug`, а также пять scoped meta-tools: `search_admin_tools`, `load_domain`, `describe_tool`, `execute_admin_read` и `execute_admin_write`. До создания бизнеса их targets ограничены состоянием, проверкой адреса и claim своего бизнеса; документационные MCP-инструменты пока недоступны. Сначала прочитайте состояние и `body.entry_context`, согласуйте адрес и язык, затем выполните claim. Следуйте `next_action` и readiness, а после их изменения снова обновите список. До появления бота не требуйте identity или каталог. Полный порядок — [новый бизнес](/docs/getting-started#new-business).

Для существующего бизнеса проверьте доступные бизнесы, их `role` (`owner` или `operator`) и активный контекст. Если нужен другой, переключайте общий контекст человека только разрешённым инструментом и снова читайте `tools/list`; это меняет контекст и в других подключениях. API key без человека не переключает этот указатель. Порядок — [существующий бизнес](/docs/getting-started#existing-business).

## Каталог и результат

Когда readiness, scope `catalog:read` и тариф допускают чтение, вызовите `get_catalog` с `{"limit":10,"zone":"ru"}`. Результат — страницы вашего каталога с внешними номерами; пустой каталог тоже допустим. Не придумывайте позиции и не считайте схему инструмента успешным вызовом. Если dedicated tool не установлен, discovery через `load_domain`/`describe_tool` не добавляет его автоматически: используйте существующий meta-dispatch согласно [MCP-руководству](/docs/mcp-guide).

Подключение подтверждено для вашей сессии только после успешного вызова и проверки нужного бизнеса. Чтение каталога не разрешает денежные операции; для записи нужны отдельные права. Номера объектов принадлежат активному бизнесу и не используются в анонимных публичных ссылках.

### Контрактный пример состояния

Сокращённая обезличенная проекция `get_onboarding_state` до первого бизнеса. Это пример серверного контракта, а не запись успешного входа из данного хоста. Текст `say_to_user` и подсказки `hint` опущены; реальный ответ используйте целиком.

```json
{"tenant":{"slug":null,"bot_username":null},"active_slug":null,"stage":"address","status":"todo","next_action":{"goal":"address","collect":[{"field":"slug"},{"field":"language"}]},"body":{"entry_context":{"intent":"auto","active_business":null,"owned_businesses":[],"recommended_business_slug":null,"action":"create_business"},"completedSteps":[],"site_operation":null,"language":"ru"}}
```

После `claim_slug` следующий шаг определяется новым ответом; опубликованный сайт сам по себе не означает готовый бот. При попытке сменить уже выданный адрес сервер возвращает `SLUG_IMMUTABLE`; сохраните исходный адрес и продолжайте readiness.

## Если подключение не удалось

При HTTP 401 пройдите вход заново; при отказе прав проверьте consent, live роль и выбранный бизнес. При `PLAN_UPGRADE_REQUIRED` проверьте тариф и открытое оплаченное окно. Если инструмента нет, обновите список и readiness; не подменяйте вход токеном другого приложения. Сбой установки на неподдерживающем клиенте не исправляется одним промптом. [Коды ошибок](/docs/errors), [права и scopes](/docs/scopes-and-permissions), [curl-диагностика](/docs/curl-device-grant).
