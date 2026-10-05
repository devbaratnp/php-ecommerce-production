# Production Release Gates

## Rule

"Implemented" is not "release-ready."

A gate is:

- PASS;
- FAIL;
- UNVERIFIED;
- NOT APPLICABLE with reason.

Do not mark PASS based on assumption.

## Gate 1 — Functional journeys

PASS requires applicable journey tests to succeed:

- browse/search -> PDP;
- variant -> cart;
- cart mutations;
- guest/auth checkout;
- payment method paths;
- confirmation;
- account/order tracking;
- admin fulfillment;
- cancellation/return/refund if supported.

## Gate 2 — Commerce integrity

Verify:

- server-authoritative price;
- stock enforcement under concurrency;
- order snapshot integrity;
- totals reconcile exactly;
- discount rules revalidate;
- duplicate submit/idempotency;
- order/payment/fulfillment states separate;
- refunds cannot exceed paid amount;
- returns restock according to policy.

## Gate 3 — Security

Verify:

- secure session config;
- session regeneration;
- authorization/ownership;
- CSRF;
- prepared statements;
- output encoding;
- uploads;
- password reset;
- rate limiting/abuse controls;
- payment webhook validation;
- no secrets in public/repo;
- production error handling;
- destructive actions not GET.

High-risk deployment may require external security review beyond this gate.

## Gate 4 — Storefront UX

Verify:

- catalog scope/navigation understandable;
- search/no-results usable;
- filters/sort state stable;
- PDP provides decision data;
- delivery/returns/cost discoverable;
- guest checkout not buried;
- validation/recovery clear;
- confirmation/post-purchase complete;
- no fake/dead controls.

## Gate 5 — Admin operations

Verify staff can:

- create/publish/update/archive products;
- adjust inventory safely;
- find/process orders;
- verify manual payments where supported;
- add tracking/fulfill;
- handle cancellation/returns/refunds;
- inspect audit/history.

No routine production task should require direct DB edits.

## Gate 6 — Accessibility

Target WCAG 2.2 AA core path.

Verify manually at minimum:

- keyboard purchase journey;
- focus visible/not obscured;
- forms labels/errors;
- dialogs/filters;
- status messages;
- zoom/reflow;
- contrast spot checks;
- touch targets;
- reduced motion;
- accessible auth.

Automated scanner result alone is not PASS.

## Gate 7 — Performance

Representative mobile/desktop pages measured.

Target good Core Web Vitals where feasible:

- LCP <= 2.5s;
- INP <= 200ms;
- CLS <= 0.1.

Also verify:

- no LCP lazy loading;
- responsive images;
- bounded product/admin queries;
- no obvious N+1;
- third-party scripts justified;
- caching/static headers appropriate.

If only lab data exists, record field performance as UNVERIFIED.

## Gate 8 — Analytics/observability

Verify:

- server logs writable/rotated;
- checkout/payment errors traceable;
- payment webhooks auditable;
- analytics ecommerce events validated if enabled;
- purchase event deduplicated;
- PII/secrets not leaked into logs.

## Gate 9 — SEO/discoverability

For public catalog sites:

- crawlable links;
- canonical URLs;
- unique titles/meta where important;
- Product/Breadcrumb structured data where applicable;
- variant strategy;
- sitemap;
- no accidental staging `noindex`/robots block;
- faceted URL strategy;
- pagination/load-more crawlability.

See `05-seo-discoverability.md`.

## Gate 10 — Deployment/configuration

Verify production:

- HTTPS;
- correct APP_URL/BASE_URL;
- secure cookies;
- debug disabled;
- secrets loaded correctly;
- writable storage permissions minimized;
- DB backups/restore plan;
- scheduled jobs/cron if required;
- email sender configured;
- webhook public URL/secret correct;
- file upload limits;
- 404/500 pages;
- logs;
- timezone behavior;
- cache invalidation/deploy strategy.

## Final report

Use:

```text
Release status: BLOCKED / READY WITH KNOWN LIMITATIONS / READY

Gate 1 Functional       PASS
Gate 2 Commerce         PASS
Gate 3 Security         PASS
Gate 4 UX               PASS
Gate 5 Admin            PASS
Gate 6 Accessibility    PASS
Gate 7 Performance      UNVERIFIED (no field data yet)
Gate 8 Observability    PASS
Gate 9 SEO              PASS
Gate 10 Deployment      PASS

Known limitations:
...
```

Do not silently convert UNVERIFIED into PASS to make the report look complete.
