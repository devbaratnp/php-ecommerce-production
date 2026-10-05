# Cart Domain and Behavior

## Cart goals

The cart is both a customer workspace and a server-side representation of intended purchase. It must stay resilient across quantity changes, authentication, stock changes, and repricing.

## Ownership models

### Option A: session guest + DB authenticated

Simple and compatible with small stores.

Guest:

```text
$_SESSION['cart']
```

Authenticated:

```text
carts + cart_items by customer_id
```

Requires a clear merge strategy at login.

### Option B: persistent anonymous cart

Use a cryptographically random cart token stored in a cookie/session and a DB cart record. When customer logs in, associate/merge it.

Useful for longer-lived carts and cross-request observability.

## Cart item identity

Cart item should reference the **purchasable variant**, not only the parent product.

A line may also store display snapshots, but the server re-fetches current:

- availability;
- price;
- option validity;
- quantity limits.

## Add-to-cart service

Inputs:

- cart owner/context;
- variant/purchasable ID;
- requested quantity.

Service validates:

- product published;
- variant active;
- required options resolved;
- quantity positive and bounded;
- stock/backorder policy;
- customer eligibility if restricted.

Then add or merge line according to cart rules.

## Quantity updates

Never trust arbitrary quantity.

Validate:

```text
min <= quantity <= business max <= available policy
```

If requested amount exceeds available stock, return an actionable result:

> Only 3 are currently available. Quantity was adjusted to 3.

or reject, according to UX/business policy.

## Remove

Support direct removal. UX MAY offer undo for accidental removal.

Server delete must still require CSRF/auth/cart ownership checks.

## Cart count

Define count semantics:

- line count; or
- total unit count.

Use one meaning consistently in header/mobile navigation.

Do not maintain a badge count in session as an independent truth that can drift from the actual cart. If cached, update it through the same cart service and verify where practical.

## Cart pricing

Each cart render should be able to produce:

```text
lines[]
subtotal
line discounts
order discounts
shipping estimate (if enough data)
tax estimate (if applicable)
fees
grand estimate
```

Clearly label estimates when final delivery/address/payment choice can change totals.

## Cart changes since last view

Detect and communicate:

- price changed;
- product unavailable;
- quantity reduced because stock changed;
- promotion expired;
- shipping eligibility changed.

Do not silently remove products without explanation if a customer can reasonably recover.

## Cart and authentication

Guest cart must not disappear simply because login is offered.

On login:

- load guest + account cart;
- merge deterministically;
- revalidate/reprice;
- store result;
- clear/retire guest source cart.

Retries must not double quantities.

## Cross-tab/cart concurrency

If users modify cart in multiple tabs, the server state is authoritative. Return updated cart totals/count in mutation responses.

For richer applications, use a cart version/updated timestamp to detect stale operations when needed.

## Mini-cart

A mini-cart is enhancement, not the only cart.

Full `/cart` route MUST remain usable.

Mini-cart should provide:

- updated items/count;
- subtotal/estimate;
- view cart;
- checkout;
- remove only if implemented safely.

## Checkout handoff

Checkout does not trust cart display state. It receives/loads cart identity and reprices/revalidates everything.

Handle empty/invalid cart explicitly.

## Cart acceptance tests

- [ ] Guest can add/update/remove.
- [ ] Logged-in customer can add/update/remove.
- [ ] Guest cart merges safely on login.
- [ ] Invalid/tampered variant IDs are rejected.
- [ ] Quantity limits/stock are enforced server-side.
- [ ] Price changes are reflected before order placement.
- [ ] Header/mini-cart/full-cart stay synchronized.
- [ ] Cart survives ordinary redirect/login flows.
- [ ] Empty state returns customer to relevant shopping paths.
