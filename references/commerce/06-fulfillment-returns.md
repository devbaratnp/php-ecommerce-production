# Fulfillment, Delivery, Returns, and Refunds

## Fulfillment model

Define what the business actually offers:

- home delivery;
- courier delivery;
- local/same-day;
- pickup;
- scheduled delivery;
- digital delivery;
- split shipment/multi-location.

Do not expose fulfillment options that cannot be operationally honored.

## Delivery eligibility

Eligibility may depend on:

- country/province/district/locality;
- postal code;
- distance/zone;
- product restrictions;
- total weight/value;
- stock location;
- order cutoff.

Use a centralized fulfillment service so PDP estimates, cart estimates, and checkout selection agree.

## Delivery promises

Prefer a customer-understandable arrival date or range when enough data exists:

```text
Expected Sep 24–26
```

rather than only:

```text
3–5 business days
```

If uncertainty is high, communicate it honestly.

Do not present a fabricated exact date when courier data only supports a broad estimate.

## Delivery cost

Expose cost/estimate early enough for product evaluation when practical.

For destination-dependent cost:

- ask/select location progressively;
- reuse known customer location with clear ability to change;
- label estimates;
- recalculate at checkout.

## Shipping threshold

If free shipping exists:

```text
Free delivery above Rs 5,000
Add Rs 850 more for free delivery
```

The progress message must derive from the same threshold/totals logic as checkout.

## Shipment tracking

Where carrier tracking exists, store:

- carrier;
- tracking number/reference;
- tracking URL only if trusted/generated safely;
- shipment events or normalized status if integrated;
- expected delivery;
- delivered timestamp.

Customer account/order detail should show useful tracking information without forcing them to hunt through email.

## Partial fulfillment

If enabled, shipment items need quantity allocation. Order UI must distinguish:

- shipped items;
- pending items;
- cancelled/refunded items.

Do not show the entire order "Delivered" when one line is still pending.

## Returns policy model

Define:

- eligible product categories;
- return window;
- condition rules;
- non-returnable products;
- return shipping/pickup responsibility;
- refund/exchange/store-credit options;
- approval process;
- restocking behavior.

The UI should explain ineligibility rather than hiding the action without context.

## Return request flow

```text
Order detail
 -> select eligible item(s)/quantity
 -> choose reason
 -> choose resolution where supported
 -> method/instructions
 -> confirm request
 -> track return state
```

Return states may include:

```text
requested
approved
rejected
in_transit
received
inspected
resolved
cancelled
```

Keep refund state separate.

## Restocking

Only restore inventory when business rules say returned items are sellable and the return has reached the appropriate inspection/receipt state.

Do not restock automatically at return-request time.

## Exchange

An exchange is not simply a note field. Decide whether it creates:

- replacement order;
- stock reservation;
- price difference handling;
- shipping fee;
- linkage to original return/order.

If the business cannot automate it, make the workflow explicit and auditable.

## Cancellation vs return

Cancellation occurs before fulfillment according to business rules.
Return occurs after shipment/delivery or another defined point.

Do not force both into one status transition.

## Customer communication

For material state changes, consider:

- order confirmed;
- payment confirmed/failed;
- shipped;
- out for delivery if data exists;
- delivered;
- return approved/rejected;
- refund initiated/completed.

Notifications must match server state and provide a valid destination back to order details.

## Fulfillment acceptance tests

- [ ] Delivery methods reflect destination/product eligibility.
- [ ] Cost/date estimate updates when relevant inputs change.
- [ ] Tracking belongs to the correct customer's order.
- [ ] Partial shipment is represented accurately if supported.
- [ ] Return eligibility/window is calculated server-side.
- [ ] Ineligible return shows reason.
- [ ] Inventory restock occurs only at the configured stage.
- [ ] Refund state is distinct from return logistics.
- [ ] Customer/admin views agree on order/fulfillment state.
