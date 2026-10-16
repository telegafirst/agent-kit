# Site page nudge

A nudge is a delayed follow-up to a person who opened a published page of the owner's site as a mini-app and left without reaching a goal. After the wait the business sends a funnel step, then optionally hands the dialog to the AI manager with the owner's brief. Configure it per page.

1. Call `site_list_pages` and pick the page by its number and title. Only pages the business published itself and did not hide are eligible; platform storefront pages are not (`SITE_NUDGE_PAGE_NOT_ELIGIBLE`).
2. Call `site_page_nudge_get` with that number to read the current setting. A page without one reads as disabled defaults.
3. Call `site_page_nudge_set` with the whole setting: `isEnabled`, `delayMinutes` (5 to 10080), `stepSeq`, `aiEnabled`, `aiPrompt`, `goalLinkCodes`. It replaces the setting and all six fields are required, so send back what you read and change only what the owner asked.
4. `stepSeq` is the number of an existing funnel step (read it with `list_chain_steps`) or `null` for no step. The step goes first, the AI second.
5. `goalLinkCodes` takes link codes (the dl-prefixed form) of links the business already has. A tap on such a link on ANY page's nudge ends the person's wait, not only on the page that started it. Any form submission, order, registration or booking of the person is a goal automatically; do not list those.
6. Enable `aiEnabled` only when the owner's plan includes the AI manager. If `aiAvailable` is false in the read result, leave it off or tell the owner to upgrade; do not retry the write. `aiEnabled: true` also needs `aiPrompt` (1 to 2000 characters, the task for the AI); without it the write is refused with `SITE_NUDGE_AI_PROMPT_REQUIRED`.
7. Read the result of the write and report the page, the wait and what is sent. Say plainly that one person waits for one nudge at a time, that opening another page replaces the wait only when that page has an enabled nudge with a step or AI and is not in its 30-day cooldown for this person, and that the person gets no nudge on the same page for 30 days after one fired.
   A running wait uses the setting snapshot taken when it started, so an edit does not change it until the person opens the same page again. Turning the nudge off (`isEnabled: false`), hiding the page or unpublishing it cancels pending waits when their timer fires: they count as `superseded` with step and AI outcome «not_configured», and nothing is sent.
8. To judge the effect, call `site_page_nudge_stats` with an optional `from` and `to` (ISO instants; default is the last 30 days). Read `fired` against `goalAfterFire` and `goalBeforeFire`; do not promise conversion from a small `fired`. `superseded` means replaced by a wait on another page, or cancelled because the nudge was turned off or the page hidden or unpublished. `stepOutcomes` and `aiOutcomes` count only waits whose timer fired (`fired` or `failed`); `superseded` waits are not in them.

**Who a nudge reaches.** When the owner asks "will a visitor who is not registered in the bot get nudged?", answer from this:

- A visit is recorded only for a person the bot knows: one who sent `/start` or allowed the bot to write to them. Everyone else is never counted and never nudged.
- In Telegram and VK, a person who opens the page for the first time without `/start` sees the messenger's own "Allow" window once per open. If they allow, the bot gets a first contact, within about eight seconds the visit is recorded, and the nudge runs as usual. If they decline, there is no visit and no nudge; the window asks again the next time they open the page.
- MAX has no such window: only people who already started the bot are nudged.
- A page opened in an ordinary browser is never counted.

Recipe for "why are there fewer visits than page opens?": that is expected. `visits` in `site_page_nudge_stats` counts only opens the bot could tie to a known person, so it is lower than the real number of opens. Do not treat the gap as an error, do not retry the write, and do not promise the owner a nudge for everyone who opened the page; if they want more reach, the lever is getting visitors to start the bot, not a setting.

If a write is refused, the error carries one recovery line: fix that field and send the whole setting again. Do not guess page numbers, step numbers or link codes.
