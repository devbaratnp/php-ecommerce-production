# Admin / Back-Office UX

## Principle

Admin UX optimizes for operational accuracy, speed, and safe decisions. It is not simply the storefront design with a sidebar.

Operators often work under time pressure, on smaller screens, with imperfect connectivity. Dense information is acceptable when hierarchy and actions remain clear.

## Information architecture

Common modules:

- Dashboard
- Orders
- Products
- Categories
- Inventory
- Customers
- Promotions
- Returns/Refunds
- Reviews/Q&A if enabled
- Reports
- Staff/Permissions
- Settings
- Audit Log

Only show modules the business actually uses.

## Dashboard

Dashboard should answer actionable questions, for example:

- orders requiring action;
- low/out-of-stock products;
- failed/pending payments;
- returns awaiting review;
- recent sales summary;
- integration failures.

Avoid a dashboard consisting only of vanity cards such as total users/products with no operational use.

## Navigation

- stable sidebar on desktop;
- compact drawer on mobile;
- active route;
- grouped sections;
- search/command affordance only if implemented;
- role-based visibility paired with server authorization.

Hiding a nav item is not authorization.

## List/table pattern

Each major entity list should consider:

- search;
- relevant filters;
- sort;
- pagination;
- result count;
- row/card identity;
- status;
- primary action;
- batch actions only where safe;
- empty/no-results/error states.

## Search

Search should target identifiers operators actually use:

Orders:
- order number;
- customer name;
- phone/email;
- tracking number.

Products:
- name;
- SKU;
- barcode.

Customers:
- name;
- phone/email.

Do not run unbounded `LIKE` queries across every column at scale.

## Filters

Use operational filters such as:

```text
Order status
Payment status
Fulfillment status
Date range
Payment method
Courier
Store/location
```

Preserve filters in URL so staff can refresh/share/bookmark where appropriate.

## Pagination

MUST use bounded queries.

Show:

```text
Showing 21–40 of 387
```

Preserve search/filter/sort.

Large data sets may use cursor pagination, but ordinary page/offset is acceptable at moderate scale with suitable indexes.

## Status badges

Use consistent label/color mapping, but never color alone.

```text
Pending
Paid
Packed
Shipped
Delivered
Cancelled
Refunded
```

Keep payment/fulfillment/order dimensions separate instead of compressing everything into one rainbow status.

## Detail page

For complex records, use a detail page rather than stuffing all actions into table rows.

Order detail should expose:

- identity/customer/contact;
- item lines;
- totals;
- payment state/history;
- fulfillment/tracking;
- timeline/audit events;
- notes;
- available actions.

## Inline actions

Inline quick status updates are useful only when the transition is low-risk and context is sufficient.

For high-risk actions (refund, cancel after payment, delete product), require detail/context and confirmation.

## Destructive action rules

- never through GET;
- CSRF protected;
- confirm with consequence;
- permission checked server-side;
- audit actor/time;
- prefer archive to destructive delete for historically referenced entities.

For especially dangerous bulk actions, require stronger confirmation.

## Forms

Admin forms should:

- group related fields;
- preserve values after errors;
- explain operational consequences;
- show server validation inline;
- avoid giant single-page forms when product complexity is high;
- autosave drafts only if conflict/recovery behavior is robust.

## Mobile admin

Do not force 12-column tables into horizontal scroll as the only experience.

For key operations, create responsive row cards exposing:

- identity;
- status;
- important amount/date;
- primary action.

Detailed data can open into a full record.

## Slow-network behavior

- avoid full-page auto-refresh loops;
- provide explicit refresh;
- show last-updated time for live-ish dashboards;
- keep forms server-rendered/resilient;
- prevent duplicate submissions;
- handle timeout with clear retry/status check.

## Permissions UX

If staff lacks permission:

- hide actions where appropriate to reduce noise;
- still enforce server-side;
- if they reach a route directly, return clear forbidden state;
- do not expose sensitive data in the HTML and merely hide with CSS.

## Audit trail

High-value events:

- staff login/security changes;
- product price changes;
- stock adjustments;
- order status changes;
- refund initiation/completion;
- coupon creation/edit;
- staff/permission changes.

Audit entry:

```text
actor
verb/action
entity
before/after or meaningful delta
reason/note where required
timestamp
request/correlation metadata where useful
```

## Admin acceptance checks

- [ ] Dashboard surfaces actions, not only vanity numbers.
- [ ] Lists have bounded pagination.
- [ ] Search/filters match operator tasks.
- [ ] Payment/order/fulfillment statuses are distinct.
- [ ] Destructive actions use POST + CSRF + permission + confirmation.
- [ ] High-risk changes are auditable.
- [ ] Mobile operators can perform key tasks without desktop-only tables.
- [ ] Sensitive data is not sent to unauthorized roles.
