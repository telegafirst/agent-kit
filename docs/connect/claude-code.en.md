# Claude Code

MCP URL: `https://mcp.telegafirst.com/api/v1/mcp`. In the app settings, check Streamable HTTP and OAuth support, then connect the server and complete authorization.

Full platform reference: `https://mcp.telegafirst.com/llms-full.txt`.
Start by reading `get_onboarding_state` with `intent: auto`; follow the current server action. For attachments and direct credentials, see [Files from your AI host](media.en.md). Confirm your host's capabilities before promising uploads or actions.

Run `claude mcp add --transport http telegafirst https://mcp.telegafirst.com/api/v1/mcp`, then complete the OAuth flow when Claude Code opens it. Keep the connection named `telegafirst` so the agent can identify it in your workspace.
