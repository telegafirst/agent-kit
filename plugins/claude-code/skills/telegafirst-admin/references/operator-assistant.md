# Operator assistant: triage, drafts, approval, send, report

Use this when the owner or an operator asks: who is waiting for an answer, who wrote during the day or the week, whom to follow up, what someone meant to buy, or how the operators answered. The tools are on the plans that include dialogs; on a plan without them say so honestly and do not look for a side door.

## Triage

1. Call `list_dialogs` with a window (24h or 7d) and a filter: awaiting_reply, awaiting_user, with_open_order or all. Each row gives the user number, the chat status, the time of the last message from the user and from the operator, how long they have waited, and the open order or abandoned cart.
2. For every user you plan to answer call `get_user_timeline`. It is the one chronology the operator sees: messages, opened products, orders and their statuses, the user asking for an operator, who took the dialog, who answered, how many minutes the user waited. Entities come as number plus title.
3. Build the list for the owner: whom to answer, whom to nudge, who intended to buy what. Say what is unknown instead of guessing.

## Drafts and approval

1. Write a personal answer per user from their timeline. Never send from a template that ignores the history.
2. Call `prepare_reply_batch` with one item per user: the user number, the text, an optional mediaRef, optional buttons. It returns the exact texts, a preparation id and a hash. Show the owner the batch in the chat.
3. Approval happens only here, in the agent chat, as a batch. Do not ask for it through Telegram and do not send anything before the owner says to send.
4. After the owner agrees call `execute_reply_batch` with the preparation id and the hash. It sends exactly what was prepared, as a reply to the user's last message, mirrored to the operator topic. Without a preparation it refuses. If the owner changes a text, prepare again.
5. A batch holds at most 50 items. A user who wrote after the preparation is refused with REPLY_BATCH_USER_WROTE_AFTER_PREPARE: the draft answered another conversation, so read the new message and prepare that user again. The other items still go out. Repeating execute for the same preparation sends only the items that failed.
6. Read the outcome per item. A failed item is reported with its code; never claim the batch went out when only part of it did.

To write first instead of answering (a greeting, an offer, a follow-up), pass `reply_to_message_id: null` on the item: it goes out as a new message and answers nothing, and a user who writes after the preparation does not refuse it. A mailing to many users is the same batch.

A heavy file shared by many users is uploaded once and reused: see the mailing recipe.

## Operator report

Call `get_operator_report` with a window. Per operator it gives the time from the request to taking the dialog, the first answer, the median and p90 answer time, the number of dialogs, and who is waiting now and for how long. Operators appear by name and username only. The platform returns data, not a verdict: judge the quality of answers yourself by reading `get_user_timeline` of sample dialogs, and state which dialogs you read.
