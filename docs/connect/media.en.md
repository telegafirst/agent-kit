# Files from your AI host

Connect MCP using your app's guide. The agent's first action is `get_onboarding_state` with `intent: auto`. It then follows the current server instruction, performs allowed actions itself and asks only for missing information. You can continue the same business in the web interface or Mini App.

For the test product, provide the whole card and a separate whole digital delivery message. Supported content includes text, media-only messages, photos, videos, PDF and other permitted documents, multiple files and supported albums. A photo is optional. The agent preserves captions, formatting and file order and verifies readiness and delivery through server checks.

When your app exposes attachments through short-lived HTTPS URLs, the agent uses `media_import_files`. A local file path, base64 or invented URL does not work. A host that can read bytes and send HTTP PUT can use `media_request_upload`, upload each file to its returned URL with the exact declared size, then finish with `media_finalize`. Use `media_get_status` for recovery. Only confirmed durable mediaRefs go into the content creation tool.

Attachment access in a browser AI depends on the host's capabilities. If it provides neither a download URL nor byte upload, upload the files in the web interface/Mini App and continue with your agent. It must not promise access, upload or delivery without a confirmed result.

You may give the agent your Telegram bot token or payment-provider credentials for the authorized typed tool of the selected business. The agent does not repeat secrets in replies or put them in documents or pages. Authorize the MCP connector in your app's settings.

After briefly answering an unrelated question, the agent returns to the current step. It stops when asked. Completion requires a separate action and confirmed server state; setup guidance then ends.
