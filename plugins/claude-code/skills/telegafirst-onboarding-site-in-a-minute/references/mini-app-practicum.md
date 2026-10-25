# Practicum pages inside a mini app

A practicum is a few step pages a visitor walks through inside Telegram, MAX or VK. Every page is a file of the bundle; the platform supplies scripts you include with `src`. Call `site_widgets_list` first: it returns the scripts, the smallest working inclusion of each and the attributes, globals and events each one reads or emits.

## Rules that hold for every variation

- Your own code goes into a FILE of the bundle and is included with `src`. An inline script is reported in the `warnings` of `site_publish` with the recipe "put it in a file", and a stricter page policy will block it later. Publishing is not refused; fix the warning and publish again.
- Include the platform scripts with a relative `src` (`/widget/miniapp.v1.js`): it works on every host of the site.
- Never write the messenger close protocol yourself. `window.TelegaMiniApp.close()` closes the window in Telegram, MAX and VK and returns false outside a mini app; any element with `data-tgf-close` does the same on click.
- Progress kept with `window.TelegaMiniApp.progress` lives in the visitor's browser. It is a hint for navigation (which step to open next), not something the business can see. A fact the business must see is a form submission.

## Variation A: a Next button and browser progress

Design three pages: step 1, step 2 and a last page. Each Next link carries the name of its step; your script file marks the step on click with `window.TelegaMiniApp.progress.set(stepName, true)`. The entry page, in its own script file, opens the first step whose `progress.get(stepName)` is null, or the last page when every step is done. The last page ends with a button that has `data-tgf-close`. This variation sends no message to the chat.

## Variation B: a form with one hidden field and a funnel step

Use it when passing a step must reach the business: it lands in the submissions, in the segment of people who submitted a form, in the CRM, and a funnel step of the form can send a message to the visitor's chat. Without a funnel step the form sends no message.

1. Call `site_form_declare` with a one-field schema: a text field named step with a human label and the hidden flag set, the allowed origin of the site and, if a message is wanted, the funnel mode with the step to run. Keep the returned form code.
2. Do not write the form markup yourself: paste `submit_snippet.html` from the `site_form_declare` reply into each step page and add one hidden input carrying the step name inside the form. Keep the consent checkbox of the snippet as it is: unticked, required, with the absolute links to the consent and privacy documents, because the consent is given to those documents. The visitor ticks it. Never pre-tick it or hide it: without it nothing is sent and publishing the page warns that the form has no consent box.
3. Include `/widget/forms.v1.js` and `/widget/miniapp.v1.js`. In your script file listen for the submitted event of the form, mark the step in the progress and open the next page; the last page closes the window.
4. Publish, read the `warnings` of `site_publish`, fix every one, and give the owner the links of every page from `site_list_pages`.

## Buttons that open the business under the inviter

`/widget/referral.v1.js` takes the invite code from the page address (the ref parameter) or from the start parameter of the mini app and sets ready links on buttons marked `data-tgf-link` with the value bot or universal, plus `data-tgf-channel` for a channel. It builds no link and neither must you.

The address carrying the ref parameter is issued by the platform: it is the link from the invite link set of a user. Never assemble it by hand and never put a code of your own in it; the platform accepts only the code it issued.

Give every anchor a fallback `href` taken from the link set of the page: until the platform has accepted a code (the visitor came without one, or the network failed) the button stays as you wrote it and must still lead somewhere.

```html
<script src="/widget/referral.v1.js" async></script>
<a data-tgf-link="bot" data-tgf-channel="telegram" href="BOT_LINK_FROM_THE_PAGE_LINK_SET">Open in Telegram</a>
<a data-tgf-link="universal" href="UNIVERSAL_LINK_FROM_THE_PAGE_LINK_SET">Choose a messenger</a>
```

Inside a mini app a bot button opens the bot with the messenger itself and closes the window. A button marked miniapp gets no link for now: the mini app does not take an invite code yet, so do not promise it to the owner. Without a code it is not hidden either and has no fallback, so put a miniapp button only on a page that is always opened with an invite code; once the platform has accepted a code, such a button with no link is hidden.
