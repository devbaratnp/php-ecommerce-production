# Journey and Acceptance Testing

## Principle

A collection of passing page screenshots is not ecommerce testing. Test complete stateful journeys and edge conditions.

## Test layers

### Unit/domain

Examples:

- price calculation;
- coupon eligibility;
- allowed order transitions;
- return eligibility;
- refund max amount.

### Integration

Examples:

- service + DB transaction;
- cart persistence;
- order placement;
- payment webhook handling;
- inventory decrement/restock.

### End-to-end/manual

Examples:

- customer purchase;
- admin fulfillment;
- return/refund;
- keyboard-only checkout;
- mobile filter/search.

## Required baseline journeys

### J1 — Browse guest purchase

```text
Home
 -> category
 -> filter
 -> PDP
 -> select variant
 -> add to cart
 -> update quantity
 -> guest checkout
 -> address
 -> delivery
 -> COD/manual/online payment path
 -> confirmation
```

Verify cart/total/order/admin data agree.

### J2 — Search purchase

```text
Search with typo/attribute
 -> suggestions/results
 -> select product
 -> PDP
 -> purchase
```

Verify query/filter state survives return navigation.

### J3 — No-results recovery

```text
Search impossible query
 -> no-results
 -> correction/category recovery
 -> product list
```

### J4 — Variant unavailable

```text
PDP
 -> select unavailable combination
 -> clear understandable state
 -> choose valid option
 -> add
```

### J5 — Last-unit concurrency

Two sessions attempt to order final stock.

Expected:

- only allowed quantity succeeds;
- losing session gets recoverable stock message;
- inventory/order state remains consistent.

### J6 — Price changes while in cart

1. add product;
2. admin/current price changes;
3. return to cart/checkout.

Expected behavior matches pricing policy and material change is surfaced before order placement.

### J7 — Duplicate submit

Simulate:

- double click;
- refresh;
- network retry.

Expected: no accidental duplicate order/charge.

### J8 — Payment failure and retry

Test supported provider sandbox/controlled failure:

- failed/cancelled;
- timeout/unknown where possible;
- retry.

Expected: state clear, no duplicate order, stock reservation behavior correct.

### J9 — Duplicate webhook

Send same valid webhook twice.

Expected: idempotent outcome.

### J10 — Returning customer

```text
Sign in
 -> cart merge
 -> checkout
 -> account
 -> order history
 -> order detail
 -> tracking
```

### J11 — Guest order tracking

Use guest ownership mechanism; verify another random order cannot be enumerated/accessed.

### J12 — Cancellation

Test eligible and ineligible statuses; stock/payment consequences match policy.

### J13 — Return/refund

```text
Delivered order
 -> return eligible item
 -> submit reason
 -> admin approve/receive
 -> refund
 -> customer sees state
```

Test expired/ineligible window too.

### J14 — Admin order processing

```text
new order
 -> verify/confirm
 -> pack
 -> tracking
 -> shipped
 -> delivered
```

Invalid reverse transitions blocked.

### J15 — Product lifecycle

```text
create draft
 -> variants/media/price/stock
 -> publish
 -> purchase
 -> edit/archive product
 -> historical order remains intact
```

## Accessibility journeys

### A1 — Keyboard purchase

No mouse:

- nav/search;
- filter;
- PDP options;
- cart;
- checkout;
- confirmation.

### A2 — Screen-reader oriented DOM review

Inspect headings, labels, names/states, live messages, dialog semantics.

### A3 — Zoom/reflow

Test critical journey at high zoom/narrow reflow; sticky UI must not cover controls.

## Mobile journeys

### M1 — Filter drawer

Open -> select -> apply -> verify applied overview -> reopen -> selections preserved -> clear.

### M2 — Keyboard checkout

On-screen keyboard open; confirm CTA/errors are not hidden.

### M3 — PDP gallery + sticky action

Swipe media, select variant, scroll sections, use sticky action, ensure bottom nav/banner do not collide.

## Security negative tests

- remove/alter CSRF token;
- access another customer's order ID;
- call admin endpoint as customer;
- tamper product price/discount/role fields;
- upload disallowed file;
- reuse password-reset token;
- use invalid webhook signature;
- open redirect attempt.

## Performance scenarios

- cold PDP on mobile throttling;
- PLP with realistic product count/page;
- autocomplete rapid typing;
- filter apply;
- cart mutation;
- admin order list with production-like row count.

## Test record

For each scenario record:

```text
ID
Environment
Preconditions
Steps
Expected
Actual
Pass/Fail/Blocked
Evidence
Issue reference
```

Use `templates/test-matrix.md`.
