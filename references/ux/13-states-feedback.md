# States, Feedback, and Recovery

## Principle

A UI designed only for successful ideal data is a prototype. Production ecommerce must define behavior when data is absent, slow, stale, invalid, unavailable, or fails.

## Universal component state model

For each interactive component evaluate applicable states:

```text
DEFAULT
LOADING
SUCCESS
EMPTY
ERROR
VALIDATION ERROR
DISABLED
UNAVAILABLE
STALE/CHANGED
PARTIAL SUCCESS
SESSION EXPIRED
NETWORK TIMEOUT
```

Not every component needs every state.

## Search

```text
idle
focused
query typing
loading suggestions
suggestions
no suggestions
network error
results
no results
```

Do not leave an infinite spinner on failed autocomplete.

## Product list

```text
loading
loaded
zero products
zero after filters
partial images failed
catalog request error
```

Zero-after-filter needs different recovery from truly empty category.

## Product page

```text
available
option required
selected variant available
variant unavailable
product out of stock
preorder/backorder
price changed
media missing
add-to-cart processing
add success
add failure
```

## Cart

```text
empty
populated
updating quantity
line unavailable
price changed
stock reduced
coupon applied
coupon invalidated
cart request error
```

When the server adjusts quantity, explain it.

## Checkout

```text
ready
field validation error
shipping recalculating
payment processing
payment requires action
payment failed
submission timeout
order success
order created but payment pending
```

The difference between "payment failed" and "we don't know yet" matters operationally.

## Account/order

```text
signed out
signed in
session expired
no orders
orders loaded
tracking unavailable
return eligible
return ineligible
refund pending
```

## Admin

```text
no data
filtered no results
save success
conflict/stale update
action forbidden
destructive action confirmation
integration unavailable
bulk partial failure
```

## Loading feedback

Use loading indicators only when work is actually pending.

For fast server-rendered navigation, do not add artificial loaders.

Skeletons should approximate final layout to avoid shift and should not imply content that may never exist.

## Success feedback

Confirm meaningful mutations:

- added to cart;
- address saved;
- order status updated;
- refund initiated.

Success message should not hide the resulting state. Update the actual UI too.

## Error message rules

Good errors are:

- specific enough to act on;
- nontechnical;
- located near affected action/field;
- persistent long enough to read;
- safe (no SQL/stack/secret details).

Bad:

```text
Error 500
Something went wrong
```

for a recoverable stock problem.

Better:

```text
Only 2 units remain. Update the quantity to continue.
```

System errors can remain generic but offer retry/support/request ID when useful.

## Optimistic UI

Use cautiously for commerce mutations.

If you update cart count immediately before server confirms, the UI must roll back/reconcile on failure.

Never optimistically display "Order placed" or "Payment successful" before authoritative server confirmation.

## Retry

Safe retries:

- search;
- catalog load;
- address quote;
- cart mutation when idempotent/reconciled.

Dangerous retries:

- order placement;
- payment capture;
- refund.

These require idempotency and state lookup, not blind repeat POST.

## Partial failure

Bulk admin actions may partially succeed. Report per-item result rather than generic "success" when some records failed.

## Session expiry

When an authenticated operation fails because session expired:

- explain;
- route to sign in;
- preserve safe intended destination/context;
- avoid losing cart.

## Feedback accessibility

Important status updates should be exposed to assistive technology without stealing focus unnecessarily.

Use suitable status/live region semantics.

## Acceptance checks

- [ ] Critical components have defined empty/error/unavailable states.
- [ ] No endless loaders after failure.
- [ ] Success updates actual state, not only toast.
- [ ] Commerce success is never shown optimistically before authoritative confirmation.
- [ ] Dangerous retries are idempotent/state-aware.
- [ ] Session-expiry recovery preserves safe context.
- [ ] Status feedback is accessible.
