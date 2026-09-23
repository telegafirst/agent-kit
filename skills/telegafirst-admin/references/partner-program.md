# Partner programme

Read current state before proposing a payout, and never invent a partner identity or invite URL.

1. Call `partner_get_balance` for withdrawable and reserved RUB amounts.
2. Call `partner_get_invite_links` for the owner's minted personal and public links. Use the returned `dl_{code}` and telegram URL; do not construct a raw messenger link.
3. If the partner identity or links are absent, ask the owner to open the partner section in the TelegaFirst bot once. Do not retry by guessing identifiers.
4. Call `partner_request_payout` only when the owner explicitly asks. It creates a pending human approval and does not pay synchronously. Do not request an amount, requisites, or partner user id: the platform derives them server-side.
