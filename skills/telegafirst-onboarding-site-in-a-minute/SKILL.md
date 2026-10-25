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

## Sign-in and diagnosis

When the host asks the owner to sign in, a connection page with a QR code opens. Before the sign-in starts, walk the owner through these steps:

1. Open the connection page in the same browser where the owner is signed in to their AI (ChatGPT, Claude, …).
2. Scan the QR or tap the button; the TelegaFirst bot opens in Telegram — tap “Confirm”.
3. Return to the connection page in the browser and do not close it: it returns the owner to the AI and finishes the sign-in.

If the plugin is installed but MCP answers 401 or `tools/list` shows no tools, the sign-in is not finished. Do not blame the network and do not retry blindly: tell the owner the sign-in is not finished, ask them to delete the connection in the AI and connect again, and lead them through the steps above from the start.

## Start with the domain

1. Call `domain_add_request` for the selected `ru` or `com` zone.
2. Give the owner admitted DNS records exactly as supplied. Screening, review or blocked responses are not ready DNS instructions; explain the next action and wait for admission. Preserve unrelated MX and TXT records.
3. Call `domain_verify` after the record is published, then use `domain_status` until the domain is active. DNS propagation can take time; report the returned reason rather than guessing.

## Build and publish

1. Prepare a complete initial bundle whose pages explain the offer, show a clear next action, and link to the business bot. The bundle is the HTML pages plus every image, video, font, CSS and JS file they use, each as its own file. No storage account of the owner is needed: the upload URLs are the platform's.
2. Call `site_request_upload` with every file in the complete bundle and retain the returned `uploadId`.
3. Upload each declared file to its returned URL with the exact declared byte size.
4. Call `site_publish` with the same host and `uploadId`, then retain its returned `attributionSnippet`.
5. Add that exact snippet to every bot-linking page, call `site_request_upload` again for the refreshed complete bundle, upload every declared file to the fresh URLs, and call `site_publish` with the new `uploadId`.
6. Confirm the deployed state with `site_get_manifest`; use `site_fetch_file` only after the manifest identifies the exact path to inspect.

## Hand the owner the page links

`site_publish` answers with **page_urls**: every HTML page that went live, each with its `url` and `links`, the page's whole set. `site_list_pages` returns the same `links` for every page at any later time. Each link has a `kind` and a `description` already worded for the owner:

- **web** opens in any browser with no sign-in, for anyone. This is the address to put on a card, a post or a site.
- **universal** is a chooser: the visitor picks a messenger among those the business connected, and the page opens there as a mini app.
- **mini_app** (one per connected messenger, with its `channel`) opens the page directly as the mini app of that messenger, for a message sent inside it.

Give the owner every link of every page and say what each is for; never guess a homepage or build a link from a path or a code. If **page_urls** is empty, read **page_urls_reason** and **page_urls_recovery**, follow the recovery, and call `site_list_pages` for the current links; do not report the site as published until the owner has a link that opens.

## Turn an HTML prototype into a bundle

A page saved from a browser or produced by a generator often inlines images, video and fonts as `data:` URLs. One HTML tag is read only up to a fixed size, so such a page is refused with `SITE_HTML_TAG_TOO_LARGE`, which names the page, tag and attribute. Inline `<style>` is fine; put JS in a bundle file — an inline `<script>` is reported as `INLINE_SCRIPT_IN_PAGE`.

1. Decode each large `data:` URL into a file with exactly the same bytes; take the type from the `data:<type>;base64,` prefix.
2. Give the file a content-addressed name such as `assets/hero.3f2a9c.png` and point the attribute at its path relative to the page.
3. Check that no local path (`file://`, a drive letter) remains and that sibling pages point at the same files.
4. Declare every file in one `site_request_upload` and list a page's files in its `references`.
5. Video (`.mp4`, `.webm`) is accepted up to the per-video size of the plan, checked before the whole-site size. On `SITE_VIDEO_TOO_LARGE` compress the video or embed it from YouTube, VK Video or Rutube.

Every refusal carries its limit in `details` and leaves the live site unchanged. `MINT_QUOTA_EXCEEDED` means the daily upload budget of the plan is spent; `MINT_QUOTA_UNAVAILABLE` means repeat the same request after `retryAfter`. Do not quote plan sizes from memory: read them from the refusal.

## Practicum pages and platform scripts

For step-by-step pages opened inside a messenger, call `site_widgets_list` for the platform scripts and read `references/mini-app-practicum.md` for the two recipes: a Next button with browser progress, and a one-hidden-field form with a funnel step. Put your own code in a file of the bundle, never inline.

## Close a section behind a password

For an offer for one client, reports or a course, never check a password in the page: anyone reads it there, and `site_publish` reports such a page. Publish the section as ordinary pages, make the password with `site_password_set`, close the folder with `site_gate_set` naming the password, and check with `site_gate_tree`. The sign-in page is ready; an own one, nested sections for several clients and closed data as a JSON file are in `references/password-protected-section.md`. The owner chooses the password and hands it to the audience. To remind a client of a password or put it into a letter to the customer, read it back with `site_password_reveal`.

## Add a lead form or checkout only when needed

- Use `site_form_declare` to declare a form before publishing its page.
- Use `site_checkout_key` only for a page that needs checkout and keep the returned key in the page configuration that the tool authorizes.
- Use `site_get_analytics` after publication to review site activity.

## Completion standard

Tell the owner the active host, the links of every page published, the bot link destination, and any DNS or content step that remains. Do not delete a deployed site unless a person has reviewed the prepared deletion scenario.
