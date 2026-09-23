# Event funnel with online booking

Use this recipe when an event offer finishes by booking a consultation, service, or slot online.

1. Discover the event, its chains, and booking capacity with `list_events`, `list_message_chains`, `booking_list_resources`, and `booking_search_availability`.
2. Create or update the event and its event-scoped catalog offer. Check the payment currency before assigning a price.
3. Build the registration, reminder, checkout, and follow-up chains. Each bound chain needs a valid entry step.
4. Bind the event workflow with `update_event`, including the event-scoped catalog positions. Keep the booking resource separate: the offer is bound to the event, while the guest selects a time from the booking flow.
5. Run `test_funnel_pipeline`. Resolve `BOOKING_NO_RESOURCE`, catalog scope or pricing errors, and every inactive or empty chain before activation.
6. Enable the event only after the dry run is clean. Tell the owner which resource guests can book and which booking conditions remain outside the dry run, such as calendar availability.
