# Files from your AI host

Connect MCP using your app's guide. The agent's first action is `get_onboarding_state` with `intent: auto`. It then follows the current server instruction, performs allowed actions itself and asks only for missing information. You can continue the same business in the web interface or Mini App.

For the test product, provide the whole card and a separate whole digital delivery message. Supported content includes text, media-only messages, photos, videos, PDF and other permitted documents, multiple files and supported albums. A photo is optional. The agent preserves captions, formatting and file order and verifies readiness and delivery through server checks.

## How a file reaches the platform

The way depends on the app your agent runs in:

| Where the agent runs | How files travel |
|---|---|
| ChatGPT | Attach the files to your message. ChatGPT itself passes them to `media_import_files` in `files`: a download URL and the file_id of each file. |
| Claude on the web, desktop or phone through a connector | Chat attachments are not passed to tools: they have no link. The agent names HTTPS links to the files in `sources`: on your site on the platform or on an allowed file host. The platform reads a file of your published site straight from its storage, with no network request; a link to another business's site is refused. Without a link, upload the file in the web interface or Mini App. |
| Claude Code, Codex, Cursor, an SDK — anywhere the agent reads files and makes HTTP requests | The agent calls `media_request_upload`, sends each file's bytes with HTTP PUT to its returned URL with the exact declared size, then finishes with `media_finalize`. |

Base64 is not supported, neither in tool parameters nor in message text. A local file path or an invented URL does not work either.

One call carries up to 10 files and links together. A large file imports in the background: `media_import_files` answers `COMMITTED` with mediaRefs, or `IMPORTING` with an `uploadId`. The agent then checks `media_get_status` until it gets `COMMITTED`, or `FAILED` with a reason code. `STAGING` means no import is running: the agent repeats the same call with the same idempotency_key.

One idempotency_key belongs to one set of files. A repeat with the same files returns the same result — except refusals caused by the source (`MEDIA_SOURCE_UNAVAILABLE`, `MEDIA_TRANSFER_TIMEOUT`): such a repeat transfers the file again; with other files the answer is `MEDIA_MANIFEST_CONFLICT` (HTTP 409 in REST). Other files need a new key.

## How to get a file back

`media_get_download_link` with the media_ref of a confirmed file of your business returns a temporary download link and the `expiresAt` moment after which it stops working; the agent then asks for a new one. In REST the same comes from `GET /api/v1/media/download-link?media_ref=…` — the parameter has the same name as the tool field. The link does not reveal where the file is stored, and the file itself does not travel through the tool. A value that is not a mediaRef gets `VALIDATION_ERROR`; a mediaRef of another business, an unconfirmed one or a missing one gets the single answer `MEDIA_SLOT_NOT_FOUND`.

Size limits depend on the plan; the free plan has no import. A refusal names its reason as a code, such as `MEDIA_TOO_LARGE` or `MEDIA_SOURCE_NOT_ALLOWED`, and reports the limit, the plan or the source address without its path. The agent must not promise access, upload or delivery without a confirmed result. Only confirmed durable mediaRefs go into the content creation tool.

You may give the agent your Telegram bot token or payment-provider credentials for the authorized typed tool of the selected business. The agent does not repeat secrets in replies or put them in documents or pages. Authorize the MCP connector in your app's settings.

After briefly answering an unrelated question, the agent returns to the current step. It stops when asked. Completion requires a separate action and confirmed server state; setup guidance then ends.
