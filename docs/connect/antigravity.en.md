# Antigravity

MCP URL: `https://mcp.telegafirst.com/api/v1/mcp`. In the app settings, check Streamable HTTP and OAuth support, then connect the server and complete authorization.

Full platform reference: `https://mcp.telegafirst.com/llms-full.txt`.
Start by reading `get_onboarding_state` with `intent: auto`; follow the current server action. For attachments and direct credentials, see [Files from your AI host](media.en.md). Confirm your host's capabilities before promising uploads or actions.

Add the Streamable HTTP URL as an MCP connector and complete OAuth. This guide intentionally gives no installation command: confirm one from `agy --help` in an environment where Antigravity is installed.

## First request to the agent

Paste this into the agent chat:

```text
Connect to the TelegaFirst MCP server and call get_onboarding_state with intent auto. Tell me which step my business is on. Before building a funnel, an event, a booking or a site, call list_playbooks, pick the matching scenario and show me a plan with the full text of every message. Change nothing until I confirm.
```

An agent without the Agent Kit still gets the build recipe from the server itself: `list_playbooks` → `get_playbook` → `get_block`.

## If Authenticate shows "MCP Error"

Antigravity registers its OAuth client dynamically, so a repeated click or retry can leave a second registration. An extra registration grants no access: without your confirmed consent in Telegram there is no sign-in. The cause depends on the client version and is not claimed here.

1. Remove the `telegafirst` entry from the raw config, save the file and add the entry again with `serverUrl`.
2. Click Authenticate once and wait for the browser window; do not click again while the dialog is open.
3. If the error remains, test the endpoint independently with the curl diagnostics and keep the HTTP status and traceId from the response.
4. Do not substitute an integration key or another application's token for the sign-in.
