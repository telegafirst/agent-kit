---
name: telegafirst-onboarding-site-in-a-minute
description: 'Use when launching a small TelegaFirst business site quickly: connect a domain, prepare pages, publish a complete bundle, and check the result.'
metadata:
  version: __PACKAGE_VERSION__
  sha: __PACK_SHA__
---

# Launch a site in a minute

Use this skill for a small business site that introduces the offer and directs visitors to the business's TelegaFirst bot. Work with the owner’s domain and publish a complete bundle, not a partial patch.

## Check the business first

Connect MCP using the package connection guide. Call `get_onboarding_state` with `intent: auto`, follow the next_action field, and finish technical onboarding before publishing. An operator role in another business does not block creating your own: use explicit `new_business: true` only when that is the owner’s intent. When the owner supplies a bot token or provider credentials, pass them only to the authorized typed tool for the selected business; never echo them or put them in site files. Keep the storefront at the business root and the standalone about page at `/bio`.

## Start with the domain

1. Call `domain_add_request` for the selected `ru` or `com` zone.
2. Give the owner admitted DNS records exactly as supplied. Screening, review or blocked responses are not ready DNS instructions; explain the next action and wait for admission. Preserve unrelated MX and TXT records.
3. Call `domain_verify` after the record is published, then use `domain_status` until the domain is active. DNS propagation can take time; report the returned reason rather than guessing.

## Build and publish

1. Prepare a complete initial bundle whose pages explain the offer, show a clear next action, and link to the business bot.
2. Call `site_request_upload` with every file in the complete bundle and retain the returned `uploadId`.
3. Upload each declared file to its returned URL with the exact declared byte size.
4. Call `site_publish` with the same host and `uploadId`, then retain its returned `attributionSnippet`.
5. Add that exact snippet to every bot-linking page, call `site_request_upload` again for the refreshed complete bundle, upload every declared file to the fresh URLs, and call `site_publish` with the new `uploadId`.
6. Confirm the deployed state with `site_get_manifest`; use `site_fetch_file` only after the manifest identifies the exact path to inspect.

## Add a lead form or checkout only when needed

- Use `site_form_declare` to declare a form before publishing its page.
- Use `site_checkout_key` only for a page that needs checkout and keep the returned key in the page configuration that the tool authorizes.
- Use `site_get_analytics` after publication to review site activity.

## Completion standard

Tell the owner the active host, the pages published, the bot link destination, and any DNS or content step that remains. Do not delete a deployed site unless a person has reviewed the prepared deletion scenario.
