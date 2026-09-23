# Webinar funnel

Build one webinar funnel in this order and verify it before enabling it.

1. Call `list_events` and `list_message_chains`. Reuse a suitable existing chain where possible. Use `search_docs` and `get_doc` instead of guessing behaviour.
2. Create the event with `create_event`. Keep it inactive until the dry run is clean. Times are configured in the Mini App; the event phase is engine-managed.
3. Create or update the confirmation, reminder, and follow-up chains with `create_message_chain` or `update_message_chain`. Put timing on a step and ensure each chain has an entry step.
4. Read catalog state with `get_catalog` or `get_catalog_item`; use `publish_entity` and poll `get_content_draft` until each event ticket is published. Event tickets must have event scope, never shop scope.
5. Bind chains, entry steps, and ticket positions with `update_event`. An omitted field stays unchanged; `null` unbinds it. Send a chain and its entry step together.
6. Run `test_funnel_pipeline` with the event and the affected chains. Resolve every reported chain, catalog, booking-resource, or start-time problem. Read `notChecked`; an otherwise clean result does not prove time-sensitive availability.
7. Only after the dry run is clean, call `update_event` with `is_active: true`. Set `is_public: true` only when the event should appear publicly. Summarize the event, bindings, offers, and verification result for the owner.
