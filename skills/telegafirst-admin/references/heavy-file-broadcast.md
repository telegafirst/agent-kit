# Heavy file to many users: upload once, send by reference

Use this for a webinar recording or any large video or file that goes to many users. Upload it once; every recipient gets the same stored file, never a fresh copy.

1. Call `media_request_upload` with the exact file name, type and size. The size limit follows the plan. PUT the exact bytes to every returned URL.
2. Call `media_finalize` with the returned upload identifier and keep the durable mediaRef. Use `media_get_status` to recover after a lost answer. Check that the answer says the file is ready for mass sending before you start; if it does not, wait and poll again.
3. Mass send to a segment or a list: `create_broadcast` with the mediaRef, then `prepare_broadcast_launch`, show the owner the summary, and after the owner confirms `execute_broadcast_launch`. Follow progress with `get_broadcast_status`.
4. Personal texts for different users with the same file: `prepare_reply_batch` then `execute_reply_batch`, passing the same mediaRef in every item.
5. Do not upload the file again for each recipient, and do not paste bytes or base64 into a message.

If your host cannot PUT bytes, say so and let the owner upload through the web or Mini App, then continue with the mediaRef.
