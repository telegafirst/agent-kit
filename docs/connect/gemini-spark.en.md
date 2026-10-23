# Gemini and Gemini Spark

MCP URL: `https://mcp.telegafirst.com/api/v1/mcp`. In the app settings, check Streamable HTTP and OAuth support, then connect the server and complete authorization.

Full platform reference: `https://mcp.telegafirst.com/llms-full.txt`.
Start by reading `get_onboarding_state` with `intent: auto`; follow the current server action. For attachments and direct credentials, see [Files from your AI host](media.en.md). Confirm your host's capabilities before promising uploads or actions.

Gemini Apps Custom MCP (web and mobile), checked against Google's official help on 2026-10-23:

1. Open the Gemini web app, then Settings → Connected Apps (or Personal Intelligence → Connected Apps).
2. Under Custom apps, add a custom app and paste the canonical MCP URL above. There is no separate mobile URL. The advanced credential fields are not needed: the server supports dynamic client registration.
3. Click Next and sign in (see below). Afterwards the app appears as connected; it may carry the server's name, for example “Telegafirst Hub Mcp”.
4. To address it in a request, type `@` and pick the app, on the web and in the Gemini mobile app. The mobile app uses the same connection and the same consent; a missing second mobile grant is not an error.

Google's terms apply and change: a personal Google Account, age 18+, a US location, Keep Activity on, English only. Adding a custom app is done in the web app. Gemini asks you to confirm write actions itself; that does not replace the owner confirmation in Telegram where the platform requires it.

What to expect: the Telegram consent card names the code-delivery address `oauth-redirect.googleusercontent.com` marked “not verified”. This is expected, not a fault, because the shared Google address serves other integrations too. The client may start with a compact set of nine tools; this is a working surface, not the whole platform: reach the rest through `search_admin_tools`, `load_domain`, `describe_tool`, `execute_admin_read` and `execute_admin_write` within the granted rights. Do not re-authorize because of the small number of tools.

Agent Kit: installing the skill package in Gemini Apps is not confirmed, and the Skills tab does not prove an install or an `ack_skills`. Do not claim a skill is installed on the model's word, and do not ask a mobile client for local file actions. Work through MCP discovery and the meta-tools instead.

Sign-in: when your AI asks you to sign in, a connection page with a QR code opens. Open it in the same browser where you are signed in to your AI, scan the QR or tap the button, then tap “Confirm” in the TelegaFirst bot in Telegram. After that, return to the connection page in the browser and do not close it: it returns you to your AI and finishes the sign-in. If the tab was closed, or the AI says “connected” but you have no tools or get 401 after sign-in, the sign-in is not finished: delete the connection in your AI and connect again.
