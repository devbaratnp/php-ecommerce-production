---
name: php-ecommerce-production
description: Use when building, implementing, auditing, fixing, hardening, or releasing a PHP 8 + MySQL/MariaDB ecommerce system. Covers architecture, PDO, auth/session security, catalog/inventory, pricing, cart, guest checkout, orders, payments, admin operations, research-informed ecommerce UX, accessibility, Core Web Vitals, analytics, SEO, testing, and release gates.
---

# PHP Ecommerce Production Skill

## Mission

Build ecommerce systems that are operationally correct, secure, understandable to customers, manageable by staff, performant on ordinary mobile devices, and resilient outside the happy path.

Do not start by coding. First determine what kind of task this is, inspect the existing system when one exists, map the commerce capabilities, and load the references relevant to the work.

---

## 1. Route the task

Classify the request into one or more modes:

### BUILD
New ecommerce project or major greenfield module.

Required workflow:
`discover -> classify store -> feature map -> domain model -> architecture -> UX contracts -> implementation -> journey tests -> release gates`

### IMPLEMENT
Approved screens or static frontend already exist and need real ecommerce behavior.

Required workflow:
`audit screens/routes -> map controls to behavior -> map behavior to domain/service -> implement shared primitives -> wire pages -> test states`

Never rebuild approved UI unless the user asks for redesign or implementation is impossible without a small UX correction.

### AUDIT
Existing codebase or storefront must be evaluated.

Required workflow:
`inspect -> document current behavior -> findings -> severity -> root cause -> recommended fix -> verification method`

Do not silently modify during an audit unless asked.

### FIX
A specific flow is broken.

Required workflow:
`reproduce -> trace request/data/state -> identify root cause -> minimal safe fix -> regression test -> adjacent-risk check`

### UX IMPROVEMENT
Improve customer experience without inventing unsupported features.

Required workflow:
`identify task/journey -> observe current behavior -> locate friction -> load relevant UX references -> recommend behavior contract -> implement if requested`

### SECURITY
Security-focused hardening or review.

Required workflow:
`threat surface -> auth/session -> authorization -> input/output -> state-changing requests -> uploads -> secrets -> logs -> headers -> dependency/config review`

### RELEASE
Pre-production or production-readiness review.

Run every applicable gate in `references/quality/04-release-gates.md`.

---

## 2. Inspect before changing

For an existing codebase, establish at minimum:

- entry points and route structure;
- PHP/bootstrap/config loading order;
- DB abstraction and transaction behavior;
- auth/session implementation;
- customer/admin role model;
- product/variant/inventory representation;
- pricing and promotion logic;
- guest and authenticated cart behavior;
- checkout/order/payment flow;
- fulfillment/status flow;
- admin operations;
- shared UI components;
- JS/AJAX endpoints;
- error/logging behavior;
- deployment environment and PHP/MySQL versions.

If docs and code disagree, treat executable behavior and schema as current reality, document the mismatch, and avoid guessing.

Read `references/architecture/01-discovery-and-mode-routing.md`.

---

## 3. Classify the ecommerce archetype

Select the closest primary type and any secondary traits:

- general single-vendor retail;
- apparel/fashion;
- grocery/FMCG;
- electronics/technical products;
- jewelry/luxury;
- food/perishable;
- B2B/wholesale;
- digital goods;
- subscription/recurring;
- hybrid online + physical store.

The archetype affects product cards, filters, variants, comparison, delivery, returns, imagery, specifications, reorder, and admin workflows.

Read `references/optional/03-vertical-patterns.md`.

---

## 4. Produce a feature map before architecture

Separate features into:

1. **Customer-facing**: discovery, search, product evaluation, cart, checkout, account, order tracking, returns.
2. **Commerce logic**: catalog, variants, price, promotions, stock, order totals, payment, fulfillment.
3. **Operations**: products, inventory, orders, customers, returns, reports, audit logs.
4. **Platform**: auth, security, files, email, logging, analytics, SEO, performance.
5. **Optional**: reviews, wishlist, compare, gifting, subscriptions, multi-location stock, B2B pricing.

Use `templates/feature-map.md`.

---

## 5. Architecture defaults

Unless the existing project intentionally uses another sound architecture:

- PHP 8.x;
- MySQL/MariaDB with InnoDB;
- PDO with native prepared statements;
- one bootstrap path for config/session/security;
- controllers/request handlers thin;
- business rules in services;
- repositories/data-access isolated from views;
- server-rendered HTML by default, JS as progressive enhancement;
- POST/Redirect/GET after successful HTML form mutations;
- transactions around order placement, stock reservation/decrement, refund bookkeeping, and other multi-write invariants;
- server authoritative for money, stock, permissions, and order state;
- immutable snapshots on order items for product name/SKU/price at purchase time;
- public identifiers separated from predictable internal IDs where exposure matters.

Read architecture references before large structural changes.

---

## 6. Non-negotiable security baseline

MUST:

- use password hashing APIs, never reversible/raw password storage;
- regenerate session ID after authentication and privilege changes;
- configure secure session cookie attributes for the real deployment flow;
- enforce authorization on the server for every privileged action;
- validate CSRF protection on state-changing browser requests using cookie auth;
- use PDO prepared statements for user-controlled values;
- escape untrusted output for the destination context;
- validate uploaded files by content/type/size and store safely;
- keep secrets outside public web roots and source control;
- log detailed errors server-side while showing safe user messages;
- never perform destructive operations through GET;
- rate-limit sensitive authentication and abuse-prone endpoints;
- use HTTPS in production and set appropriate security headers.

Load `references/security/` for implementation details.

---

## 7. Commerce integrity rules

The browser is never authoritative for:

- price;
- discount eligibility;
- shipping charge;
- tax;
- stock;
- maximum quantity;
- payment success;
- order status;
- role/permission.

At checkout, re-read products/variants/prices/stock from the server, calculate totals centrally, persist order snapshots, and protect the write against duplicates.

Money displayed to the customer must be traceable to the same source used for order calculation.

Read `references/commerce/`.

---

## 8. UX behavior is a contract

Every important page or component should define:

- required data;
- user decisions/actions;
- valid and invalid states;
- loading/empty/error/unavailable behavior;
- mobile behavior;
- accessibility behavior;
- performance constraints;
- analytics events where appropriate.

Use `templates/page-contract.md`.

A page is not complete because its ideal-state screenshot matches the design.

---

## 9. Storefront principles

MUST:

- make search/browse/cart/account entry points real destinations;
- support meaningful browsing without requiring exact keywords;
- preserve product-list context when users filter, sort, paginate, visit a PDP, and return;
- expose enough product information for informed decisions;
- show real availability, cost, delivery, returns, and support information;
- support guest checkout unless a documented business/regulatory requirement truly prevents it;
- make post-purchase order information and self-service discoverable;
- avoid dead controls and fake commerce signals;
- keep essential actions usable with keyboard, touch, and without JavaScript where reasonable.

Load the relevant `references/ux/` modules.

---

## 10. Guest checkout rule

Do not require account creation as a default ecommerce prerequisite.

Preferred flow:

`Cart -> Guest checkout / Sign in -> Contact & delivery -> Payment -> Review/Place order -> Confirmation -> Optional account creation`

If account creation is required by the business model, explain that constraint before the user has invested deeply in checkout.

Guest carts and guest orders must remain recoverable according to the project's privacy/security model.

---

## 11. Admin/back-office rule

Admin UX is not a styled CRUD table. Operators need to answer questions and take actions safely.

Admin modules should optimize for:

- finding the right record quickly;
- seeing the status/context needed for a decision;
- preventing destructive mistakes;
- preserving an audit trail;
- handling mobile/slow connections when staff need it;
- avoiding unbounded database queries;
- processing orders, inventory, returns, and customers without manual DB edits.

Load `references/admin/`.

---

## 12. Accessibility and mobile are release requirements

Target WCAG 2.2 AA for the core purchase and self-service journeys.

Use semantic/native controls before custom widgets. Fixed/sticky UI must not obscure focused controls. Status changes such as cart updates must be programmatically exposed. Authentication must remain compatible with password managers and copy/paste.

Mobile is a behavioral override, not merely a smaller CSS breakpoint. Define filter sheets, menu behavior, sticky actions, keyboard behavior, image interaction, and checkout inputs explicitly.

Load `references/ux/10-mobile-responsive.md` and `11-accessibility.md`.

---

## 13. Performance is UX

Production target at the 75th percentile where field data is available:

- LCP <= 2.5 s
- INP <= 200 ms
- CLS <= 0.1

Do not lazy-load the likely LCP product/hero image. Reserve media dimensions. Avoid rendering huge unbounded product lists. Measure real-user performance after launch where possible.

Load `references/ux/12-performance.md`.

---

## 14. Analytics and observability

The system should be debuggable and measurable without collecting unnecessary personal data.

At minimum consider:

- product/list/search/cart/checkout/purchase events;
- server-side order/payment logs with correlation IDs;
- payment webhook audit trail;
- admin action audit log for sensitive changes;
- exception/error logging;
- RUM/Core Web Vitals where appropriate.

Do not log passwords, full payment credentials, session IDs, or unnecessary personal data.

Load `references/quality/03-analytics-observability.md`.

---

## 15. Testing rule

Before declaring a feature complete, test applicable journeys rather than isolated pages.

Minimum release journeys normally include:

- browse -> PDP -> cart -> guest purchase;
- search (including no-result recovery) -> PDP -> purchase;
- variant/stock edge cases;
- cart quantity/update/remove;
- checkout validation and duplicate-submit protection;
- returning customer sign-in -> order history -> tracking;
- admin order processing;
- return/refund if supported;
- keyboard-only core purchase path;
- narrow/mobile viewport core purchase path;
- degraded/error states for critical APIs/payment calls.

Load `references/quality/02-journey-tests.md`.

---

## 16. Release gates

Do not say "production ready" unless all applicable gates pass:

1. Functional journeys
2. Commerce integrity
3. Security
4. Storefront UX
5. Admin operations
6. Accessibility
7. Performance
8. Analytics/observability
9. SEO/discoverability where public catalog traffic matters
10. Deployment/configuration

If a gate is untested, mark it **UNVERIFIED**, not passed.

Read `references/quality/04-release-gates.md`.

---

## 17. Reference loading guide

### New project
Load architecture -> security -> commerce -> relevant UX -> admin -> quality.

### Product page
Load:
- `references/ux/05-product-page.md`
- `references/ux/06-product-media-variants.md`
- `references/ux/07-reviews-qa-trust.md` if reviews/Q&A exist
- `references/ux/11-accessibility.md`
- `references/ux/12-performance.md`

### Search / PLP
Load:
- `references/ux/02-homepage-category-taxonomy.md`
- `references/ux/03-search-autocomplete.md`
- `references/ux/04-plp-filter-sort.md`
- mobile/accessibility references.

### Checkout
Load:
- `references/commerce/03-cart.md`
- `references/commerce/04-checkout-orders.md`
- `references/commerce/05-payments.md`
- `references/ux/08-cart-checkout-forms.md`
- mobile/accessibility/security references.

### Account/order tracking/returns
Load:
- `references/commerce/06-fulfillment-returns.md`
- `references/ux/09-account-self-service-post-purchase.md`
- auth/accessibility references.

### Audit
Load:
- `references/quality/01-audit-framework.md`
- then only the modules implicated by findings.

---

## 18. Completion language

Use precise completion states:

- **implemented**: code exists;
- **verified**: behavior has been tested;
- **release-ready**: all applicable release gates passed;
- **unverified**: implementation exists but required validation was not performed;
- **not applicable**: requirement was consciously excluded with reason.

Never claim a project is secure, accessible, or production-ready solely from code inspection when runtime/deployment checks are required.
