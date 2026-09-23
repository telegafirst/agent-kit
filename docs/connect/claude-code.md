## MCP connection prompt — TODO(190-12-15-07)

1. MCP URL: `https://mcp.telegafirst.com/api/v1/mcp`
2. Connect it as a connector; OAuth completes when you add it.
3. Full platform description: `https://mcp.telegafirst.com/api/v1/external/llms-full.txt`
4. Task: help me configure and operate my TelegaFirst AI front office.

# Claude Code

Run `claude mcp add --transport http telegafirst https://mcp.telegafirst.com/api/v1/mcp`, then complete the OAuth flow when Claude Code opens it. Keep the connection named `telegafirst` so the agent can identify it in your workspace.
