# Booking service: length, price ladder, add-ons

Use this recipe to set up a bookable service whose length the buyer chooses (a sauna by the hour, a cabin by the night) or to place a booking for a person.

1. Read the resources with `booking_list_resources`. For several identical places (ten cabins, five chairs) create them in a loop with `booking_create_resource`, each call with its own idempotency key and a name such as "Cabin 1" … "Cabin N"; there is no separate "multiply" command. Give each resource a schedule with `booking_create_resource_schedule`.
2. Create the service once with `booking_create_service`, passing every resource number and a base price. Choose the length in the service length object:
   - length mode fixed is one block (a haircut).
   - length mode list with the allowed block counts is one of them (2, 4 or 8 hours).
   - length mode range with a minimum and a maximum count is any count in between; an hourly service may also set the pickup and return window in minutes from midnight in the business timezone.
   Without a length object the service is a range, linear by the block price. Change an existing service with `update_catalog_item`.
3. A price ladder goes into the price tiers of the length object: each step has an id, a unit count and prices, and means a package of that many blocks costs that price. Steps ascend by units, the step price never falls, and every step carries the currencies of the base price. Ask the owner for the package prices; never invent them.
4. Search with `booking_search_availability`. A slot is one block. The reply carries the service length (mode, allowed counts or minimum and maximum, the window): size the booking from it.
5. Place the booking with `booking_create`: send the end of the range and let the platform count the blocks, or send a quantity. Add-ons the service offers go into the selected options as an attribute key with the chosen option keys; the order quote prices them.
6. Read the refusals: QUANTITY_LIMIT means the length is outside what the service offers, OUTSIDE_WINDOW means the start or the end falls outside the pickup and return hours. Both name what to change; send a corrected request, not the same one again.
7. The order of a booking consists of its length and add-ons, so editing its lines answers ORDER_BOUND_TO_BOOKING. Move the booking with `booking_reschedule`, or cancel it with `booking_cancel` and create it again with the intended end and selected options.
