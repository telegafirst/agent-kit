# Codex

MCP URL: `https://mcp.telegafirst.com/api/v1/mcp`. In the app settings, check Streamable HTTP and OAuth support, then connect the server and complete authorization.

Full platform reference: `https://mcp.telegafirst.com/llms-full.txt`.
Start by reading `get_onboarding_state` with `intent: auto`; follow the current server action. For attachments and direct credentials, see [Files from your AI host](media.en.md). Confirm your host's capabilities before promising uploads or actions.

Add the Streamable HTTP URL as an MCP connector and complete OAuth. After connecting, ask the agent to read the relevant skill through MCP and acknowledge the current package with `ack_skills` when needed.
