# Mailing to many users: personal batch first, heavy files by reference

Use this when the owner asks for a mailing, a greeting, an offer or a follow-up to many users, with or without a large video or file such as a webinar recording.

Choosing the path: a personal text for each customer goes through prepare_reply_batch then execute_reply_batch; one identical message to a whole segment goes through broadcasts (create_broadcast).

Default to the personal batch. Use the broadcast only when the owner asks for one identical message to a segment.

## Personal text for each user (the default)

1. Find the recipients. `sample_segment_audience` gives the users of a segment filter, each with marketingConsent (true granted, false refused advertising, null no record); `list_dialogs` gives the users who wrote. Read `get_user_timeline` for the ones you write to, so each text uses what that user did.
2. Write a personal text per user, never one template with a name pasted in. Call `prepare_reply_batch` with one item per user. To write first (a greeting, an offer, a follow-up) pass `reply_to_message_id: null` on the item: it goes out as a new message and answers nothing. Leave it out to answer the user's last message. A batch holds at most 50 items; split a longer list into several batches.
3. Show the owner the exact batch from the answer. A batch does not skip customers who opted out of promotions, because it carries personal messages and not promotion: promotional text to someone who opted out is at the owner's own risk. When the text is promotional, count the recipients whose marketingConsent is false and tell the owner that number before they confirm; leave them out of the batch if the owner asks. After the owner confirms, call `execute_reply_batch` with the preparation id and the hash. Without the owner's confirmation nothing is sent.
4. Read the outcome per item and tell the owner exactly what it says. Each message states its delivery: delivered (the messenger confirmed it), queued (in the sending queue, not delivered yet), unknown (the messenger may have accepted it and no answer will follow) or failed (it did not go out; the element is refused with the reason). Only delivered means it reached the customer: tell the owner that a queued message is waiting and that an unknown one may or may not have arrived, and never send either again.

## One identical message to a segment

`create_broadcast` with the content and the segment filter, then `prepare_broadcast_launch`, show the owner the summary, and after the owner confirms `execute_broadcast_launch`. Follow progress with `get_broadcast_status`.

## A heavy file to many users

Upload it once; every recipient gets the same stored file, never a fresh copy.

1. Call `media_request_upload` with the exact file name, type and size. The size limit follows the plan. PUT the exact bytes to every returned URL.
2. Call `media_finalize` with the returned upload identifier and keep the durable mediaRef. Use `media_get_status` to recover after a lost answer. Check that the answer says the file is ready for mass sending before you start; if it does not, wait and poll again.
3. Pass the same mediaRef in every item of the personal batch, or in the broadcast content.
4. Do not upload the file again for each recipient, and do not paste bytes or base64 into a message.

If your host cannot PUT bytes, say so and let the owner upload through the web or Mini App, then continue with the mediaRef.
