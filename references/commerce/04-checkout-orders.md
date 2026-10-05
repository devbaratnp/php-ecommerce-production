# Checkout and Order Creation

## Core principle

Checkout converts mutable intent (cart) into an auditable historical order. The server must revalidate all commerce facts at the boundary.

## Default customer flow

```text
Cart
 -> Guest checkout / Sign in
 -> Contact + delivery address
 -> Delivery method
 -> Payment method/details
 -> Review / Place order
 -> Confirmation
 -> Optional account creation/linking
```

Steps may be combined for small stores. The UI progress indicator must match the actual steps.

## Do not require login by default

Guest checkout is supported unless a real business/regulatory rule requires an account.

If login is optional, do not visually bury guest checkout below sign-in forms.

## Checkout session/state

Keep checkout state server-side or validate all client-submitted state.

Never trust browser values for:

- unit price;
- discount;
- shipping cost;
- tax;
- stock;
- grand total;
- payment success.

## Contact and address

Collect only what fulfillment/payment/legal needs.

Store an immutable order snapshot separate from a customer's editable saved address.

If customer later edits their account address, existing orders must not change.

## Delivery method

A delivery option should provide:

- label;
- cost;
- expected arrival/date range when possible;
- restrictions;
- selectable state.

Recalculate totals after delivery choice.

## Place-order transaction

Conceptual flow:

1. validate checkout input;
2. load current cart lines;
3. fetch current products/variants/prices;
4. validate promotion eligibility;
5. validate stock;
6. calculate authoritative totals;
7. begin transaction;
8. lock/revalidate stock as needed;
9. create order;
10. create immutable order items;
11. create payment intent/attempt record as appropriate;
12. reserve/decrement stock according to policy;
13. persist totals/address/contact/delivery snapshots;
14. commit;
15. trigger post-commit email/jobs/integrations;
16. clear/close cart only when correct for the payment flow.

For online payments, the exact order/payment sequencing depends on provider. Do not hold a DB transaction open while waiting for customer/provider network interaction.

## Idempotency

A customer may double-click, refresh, retry after timeout, or a webhook may be delivered twice.

Order/payment actions should support idempotency.

Possible approach:

- generate checkout submission key server-side;
- persist/consume it atomically;
- if a completed operation exists for the key, return the existing result rather than create another order.

Payment providers may supply their own idempotency keys; use them according to provider docs.

## PRG

For synchronous COD/manual-payment forms:

```text
POST /checkout/place
 -> create order
 -> set success reference
 -> 303/redirect to /order-confirmation/{token}
```

Refreshing the confirmation page must not create another order.

## Order number

Use a customer-friendly public order identifier distinct from a raw sequential primary key when practical.

Do not expose enough information to let users enumerate other customers' orders.

## Order states

Model payment and fulfillment separately.

Example business order state:

```text
pending -> confirmed -> processing -> completed
       \-> cancelled
```

Fulfillment:

```text
unfulfilled -> packed -> shipped -> delivered
```

Payment:

```text
unpaid -> pending -> paid
                 \-> failed
paid -> partially_refunded -> refunded
```

Exact states are project-specific.

## Confirmation

Order confirmation should be based on persisted order state and expose:

- order number;
- purchased lines;
- contact/delivery summary;
- totals;
- payment method/state;
- expected next step;
- support route;
- account/order tracking path.

Do not claim online payment is successful until the server has authoritative confirmation according to the provider flow.

## Email

Send after the order transaction is committed. Prefer queue/background processing where infrastructure permits, but do not make order success dependent on mail server success.

If synchronous only, catch/log email failure separately from the committed order.

## Cancellation

Define:

- which statuses can be cancelled;
- who can cancel;
- whether stock is restored;
- whether payment void/refund is needed;
- audit trail;
- customer notification.

Do not let an admin arbitrarily move a shipped order back to pending to represent cancellation.

## Checkout acceptance tests

- [ ] Guest can complete supported checkout.
- [ ] Browser-tampered totals are ignored.
- [ ] Stock is checked at placement boundary.
- [ ] Two submissions cannot create duplicate order unexpectedly.
- [ ] Order items preserve purchase-time data.
- [ ] Address snapshot is immutable from account edits.
- [ ] Cart clears at the correct lifecycle point.
- [ ] Refreshing confirmation does not resubmit.
- [ ] Payment/fulfillment/order states remain distinct.
- [ ] Confirmation reflects persisted server state.
