# System Architecture

## Architectural goal

Keep rendering, HTTP handling, business rules, and database concerns distinct enough that ecommerce invariants are testable and reusable across storefront/admin/API entry points.

This is not a demand for an enterprise framework. Plain PHP can remain simple while still separating responsibilities.

## Recommended project shape

```text
app/
├── Controllers/
├── Services/
├── Repositories/
├── Domain/
├── Policies/
├── Support/
└── Validation/
config/
├── app.php
├── database.php
└── services.php
public/
├── index.php
├── assets/
└── uploads/   # only if safe public derivatives live here
routes/
├── web.php
└── admin.php
views/
├── storefront/
├── account/
├── admin/
└── components/
storage/
├── logs/
├── private-uploads/
└── cache/
database/
├── migrations/
└── seeds/
```

Existing projects do not need a forced rewrite to match this exact tree. Preserve stable structure when it already enforces the same boundaries.

## Request lifecycle

Preferred HTML request flow:

```text
Browser
  -> public entry/router
  -> bootstrap
  -> authentication/authorization
  -> controller/handler
  -> validation
  -> service
  -> repository/DB
  -> service result
  -> view
```

POST mutation:

```text
POST
 -> validate auth/CSRF/input
 -> service transaction
 -> flash result
 -> redirect
 -> GET renders resulting state
```

Use PRG for ordinary form mutations so browser refresh does not repeat a purchase/update.

## Bootstrap contract

One bootstrap should establish:

- environment/config;
- constants/URL helpers;
- error/log behavior;
- PDO connection factory;
- secure session settings before `session_start()`;
- session start;
- CSRF/session helpers;
- security headers;
- autoload/shared services.

Do not define constants after files that use them.

## Database connection

Default PDO characteristics:

```php
$pdo = new PDO($dsn, $user, $pass, [
    PDO::ATTR_ERRMODE            => PDO::ERRMODE_EXCEPTION,
    PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
    PDO::ATTR_EMULATE_PREPARES   => false,
]);
```

Use utf8mb4 at the DB/connection/table level.

Do not expose raw connection exceptions to customers.

## Controller rules

Controllers/handlers SHOULD:

- parse request input;
- invoke auth/policy checks;
- validate shape;
- call services;
- choose redirect/view/JSON response.

Controllers SHOULD NOT:

- contain complex stock allocation;
- calculate final order totals independently;
- write five related tables manually;
- duplicate price rules from admin/storefront;
- decide authorization from hidden form fields.

## Service rules

Services own business use cases, for example:

- `AddToCartService`
- `PriceCartService`
- `PlaceOrderService`
- `ApplyCouponService`
- `CancelOrderService`
- `TransitionOrderStatusService`
- `RequestReturnService`
- `AdjustInventoryService`

Services should be usable from HTML controllers, admin controllers, and API endpoints without rewriting rules.

## Repository/data access rules

Repositories isolate SQL and mapping.

They SHOULD:

- use prepared statements;
- return explicit domain/data shapes;
- expose operations needed by services;
- avoid hidden business side effects.

They SHOULD NOT decide whether a coupon is valid, an order can be cancelled, or a user is authorized. Those are domain/service/policy decisions.

## Transaction boundaries

Use a transaction whenever several writes must succeed or fail together.

Typical boundaries:

- order + items + totals + stock reservation/decrement;
- payment capture bookkeeping + order payment state;
- refund + payment ledger + order/refund state;
- inventory transfer between locations;
- return acceptance + restock + refund record where synchronized.

Example pattern:

```php
$pdo->beginTransaction();
try {
    // lock/read authoritative rows as needed
    // validate invariant
    // write related records
    $pdo->commit();
} catch (Throwable $e) {
    if ($pdo->inTransaction()) {
        $pdo->rollBack();
    }
    throw $e;
}
```

Do not keep DB transactions open while waiting for a remote payment API response. Split local state transitions around external calls and use idempotency/reconciliation.

## Server-rendered first

For ordinary storefronts, server-rendered HTML is a strong baseline:

- real URLs;
- resilient navigation;
- simpler auth/security;
- discoverability;
- usable fallback when JS fails;
- lower client complexity.

Use JavaScript to enhance search suggestions, cart feedback, drawers, galleries, quantity controls, and payment SDKs, not to turn every link into an opaque JS event.

## URL and routing standards

- canonical public resource URLs should be stable;
- product/category links must be real anchors;
- GET is safe/read-only;
- POST/PATCH/DELETE semantics for mutation according to project router capabilities;
- never delete/change state through a GET link;
- preserve return/intended destination safely after auth;
- validate redirect destinations against local allowlists to prevent open redirects.

## Shared UI components

Create shared components for recurring behavior:

- product card;
- money display;
- stock badge;
- variant selector;
- quantity control;
- pagination;
- flash/alert;
- form field + errors;
- modal/dialog/sheet;
- breadcrumbs;
- admin table/list shell;
- status badge.

A shared component must not hide business queries inside view files. Prepare data before rendering.

## Error strategy

Separate:

1. **validation errors**: user can correct;
2. **business-rule errors**: e.g. stock changed;
3. **authorization errors**;
4. **not found**;
5. **system/infrastructure errors**.

Show users actionable safe messages. Log the technical exception with a request/correlation ID.

## Configuration

Environment-specific values:

- DB credentials;
- app URL;
- debug mode;
- mail credentials;
- payment keys;
- webhook secrets;
- session cookie security;
- upload limits;
- analytics IDs;
- object storage/CDN config.

Never commit production secrets.

## Architecture acceptance checks

- [ ] One clear bootstrap path exists.
- [ ] Views do not execute arbitrary DB writes.
- [ ] Final prices and totals are calculated centrally.
- [ ] Critical multi-write operations have transaction boundaries.
- [ ] Authorization is server-side and reusable.
- [ ] Public links are real URLs.
- [ ] Errors are safe for users and detailed in logs.
- [ ] Configuration/secrets are separated from code.
- [ ] No duplicate order/cart business logic across multiple pages.
