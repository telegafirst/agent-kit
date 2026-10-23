# ChatGPT

MCP URL: `https://mcp.telegafirst.com/api/v1/mcp`. In the app settings, check Streamable HTTP and OAuth support, then connect the server and complete authorization.

Full platform reference: `https://mcp.telegafirst.com/llms-full.txt`.
Start by reading `get_onboarding_state` with `intent: auto`; follow the current server action. For attachments and direct credentials, see [Files from your AI host](media.en.md). Confirm your host's capabilities before promising uploads or actions.

Add the Streamable HTTP URL as an MCP connector and complete OAuth. For a Custom GPT or Project, you may also use the platform description URL above as instructions; skill content remains available through MCP.

Sign-in: when your AI asks you to sign in, a connection page with a QR code opens. Open it in the same browser where you are signed in to your AI, scan the QR or tap the button, then tap “Confirm” in the TelegaFirst bot in Telegram. After that, return to the connection page in the browser and do not close it: it returns you to your AI and finishes the sign-in. If the tab was closed, or the AI says “connected” but you have no tools or get 401 after sign-in, the sign-in is not finished: delete the connection in your AI and connect again.
