## MCP connection prompt — TODO(190-12-15-07)

1. MCP URL: `https://mcp.telegafirst.com/api/v1/mcp`
2. Connect it as a connector; OAuth completes when you add it.
3. Full platform description: `https://mcp.telegafirst.com/api/v1/external/llms-full.txt`
4. Task: help me configure and operate my TelegaFirst AI front office.

# TelegaFirst Agent Kit

Skills for a TelegaFirst business owner's agent: operate a Telegram AI front office, launch funnels, resolve account issues, manage partner work, and publish a small business site.

## Included skills

- `telegafirst-admin` is the operational navigation hub and its detailed playbooks.
- `telegafirst-onboarding-site-in-a-minute` launches a small business site with existing site tools.

Use the URL above as a remote MCP connector. A client that can send only HTTP headers may use its issued API key beginning with `tgf_api_`; never put keys in documents, prompts, or repositories.

For host-specific steps, open the corresponding file in `docs/connect/`.
