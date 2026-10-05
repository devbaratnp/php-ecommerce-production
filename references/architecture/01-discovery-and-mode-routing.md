# Discovery and Mode Routing

## Goal

Prevent premature implementation. Before writing code, understand whether the task is greenfield, implementation of approved screens, audit, repair, UX improvement, security hardening, or release validation.

## Step 1: identify mode

Use these modes from `SKILL.md`:

- BUILD
- IMPLEMENT
- AUDIT
- FIX
- UX IMPROVEMENT
- SECURITY
- RELEASE

A task can combine modes, but one should be primary.

## Step 2: inventory the existing project

For an existing repository, inspect rather than assume:

### Runtime

- PHP version and extensions;
- web server (Apache/Nginx/etc.);
- database type/version;
- Composer dependencies if present;
- environment/config loading;
- production vs local differences.

### Request/bootstrap

Find:

- the first included bootstrap/config file;
- session start location;
- BASE_URL or URL helpers;
- error/log settings;
- autoloader/service includes;
- security header setup.

Multiple competing bootstraps are a risk. Map them before consolidating.

### Authentication and authorization

Map:

- customer login/register/logout;
- admin/staff login;
- session variables;
- roles/permissions;
- password reset;
- redirect-after-login behavior;
- account/profile endpoints;
- admin guards.

### Commerce data

Identify tables/entities for:

- products;
- variants/options;
- categories;
- media;
- inventory/stock locations;
- carts/cart items;
- customers/addresses;
- orders/order items;
- payments;
- shipments/fulfillment;
- promotions/coupons;
- returns/refunds;
- audit logs.

Do not infer missing constraints from table names. Read schema/indexes/foreign keys.

### Customer routes and pages

Map actual routes for:

- home;
- category/PLP;
- search;
- PDP;
- cart;
- checkout;
- login/register;
- account/orders;
- order detail/tracking;
- return/support;
- content pages.

### Admin routes

Map products, categories, inventory, orders, customers, promotions, reports, settings, staff, and logs.

### UI behavior map

For each visible control answer:

- What action does the user expect?
- Is there a real endpoint/state transition behind it?
- What happens without JavaScript?
- What happens when the request fails?
- Is the control shown only when the action is allowed?

## Step 3: classify the store

Record:

- primary vertical;
- catalog size;
- product complexity;
- variant complexity;
- stock model;
- fulfillment model;
- payment methods;
- geography/currency;
- customer types;
- order frequency;
- staff/operator profile;
- device/network constraints.

This determines which optional patterns to activate.

## Step 4: produce current-state and target-state maps

A small system map is more useful than a vague plan.

```text
Current:
PLP -> PDP -> session cart -> login required -> checkout -> COD order

Target:
PLP -> PDP -> guest/auth cart -> guest checkout -> delivery choice -> COD/online payment -> confirmation -> guest tracking
```

Also map admin lifecycle:

```text
New -> Confirmed -> Processing -> Packed -> Shipped -> Delivered
                 \-> Cancelled
Delivered -> Return requested -> Returned -> Refunded/Exchanged
```

Use project-specific states rather than copying this lifecycle blindly.

## Step 5: identify invariants

Before implementation, write down what must never become false.

Examples:

- an order total cannot be trusted from the browser;
- stock cannot go below allowed policy unless backorders are explicitly supported;
- a customer cannot view another customer's private order;
- payment success cannot be accepted solely from a redirect query parameter;
- deleting a category must not orphan products unexpectedly;
- a refunded amount cannot exceed the captured amount;
- an order item must preserve its purchase-time product identity/price even if the catalog changes later.

These invariants drive transaction boundaries and tests.

## Step 6: plan by dependency order

For BUILD/IMPLEMENT, prefer:

1. bootstrap/config/database;
2. shared auth/security primitives;
3. domain/schema migrations;
4. repository/service foundations;
5. catalog/inventory/pricing;
6. cart;
7. checkout/orders;
8. payments/fulfillment;
9. customer account/self-service;
10. admin operations;
11. analytics/SEO/performance polish;
12. release tests.

Do not wire a polished checkout UI to placeholder order logic and call it complete.

## Audit output format

Every material finding should include:

- **Finding**: concise statement;
- **Location**: route/file/component;
- **Evidence**: observed behavior/code/data;
- **Impact**: user/business/security consequence;
- **Severity**: Critical/High/Medium/Low;
- **Fix**: behavior/architecture change;
- **Verification**: how to prove the issue is resolved.

See `references/quality/01-audit-framework.md`.
