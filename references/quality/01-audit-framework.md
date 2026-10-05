# Ecommerce Audit Framework

## Purpose

Audit systematically. Do not produce a random list of visual opinions.

An audit should connect evidence -> impact -> severity -> fix -> verification.

## Audit scopes

Choose applicable scopes:

1. architecture/code health;
2. security;
3. catalog/inventory integrity;
4. storefront UX;
5. checkout/payments;
6. account/self-service;
7. admin/operations;
8. accessibility;
9. performance;
10. analytics/observability;
11. SEO/discoverability;
12. deployment/configuration.

## Evidence hierarchy

Strong evidence:

- reproduced behavior;
- source code/schema/config;
- logs/query output;
- automated test result;
- browser/network/accessibility inspection.

Weaker evidence:

- screenshot only;
- assumptions from route names;
- stale documentation.

If evidence is incomplete, mark finding as provisional/unverified.

## Severity model

### Critical

Issue can directly cause severe security/data/financial failure or make core purchase impossible broadly.

Examples:

- SQL injection/auth bypass;
- payment marked successful without verification;
- users can access others' orders;
- duplicate order charge path;
- stock corruption with no recovery.

### High

Substantial user/business risk or major funnel blocker.

Examples:

- checkout inaccessible on mobile;
- guest option hidden/absent without business reason;
- add-to-cart regularly fails;
- admin cannot process paid orders;
- severe accessibility blocker in checkout.

### Medium

Meaningful friction/error but recoverable.

Examples:

- filters lost on sort;
- shipping info hard to locate;
- order list lacks enough identification;
- slow PDP image causing poor LCP.

### Low

Localized polish/consistency issue with low task impact.

Do not inflate everything to High.

## Optional second dimension: frequency

Record:

- Always;
- Frequent;
- Occasional;
- Edge case;
- Unknown.

Priority can be derived from severity × frequency × business importance.

Do not invent revenue impact percentages unless real analytics supports them.

## Finding format

```markdown
### [HIGH] Applied filters disappear after sorting

**Location:** `/products?category=phones`, Sort control

**Evidence:** Selecting Brand=Samsung then changing sort to Price Low-High drops `brand` from the URL and restores all brands.

**Impact:** Users lose narrowing work and may believe results are inconsistent.

**Root cause:** Sort form serializes only `sort`, not current query/filter state.

**Fix:** Build sort URLs/forms from canonical query-state object; preserve all active filters and query, reset only `page=1`.

**Verification:** Apply 3 filters -> sort -> paginate -> refresh -> browser back. Filter state remains identical.
```

## Storefront journey audit

Audit by journeys, not only pages.

### Discovery

- new customer understands catalog;
- browse categories;
- search exact + type + attribute;
- recover from zero results.

### Evaluation

- PLP filter/sort;
- PDP variant/media/spec/cost/delivery/returns;
- reviews if enabled.

### Purchase

- add/update/remove cart;
- guest checkout;
- delivery choice;
- payment;
- duplicate submission;
- confirmation.

### Post-purchase

- order list/details;
- guest tracking;
- cancellation;
- returns/refunds;
- reorder.

## Security audit checklist

- session config/regeneration;
- auth enumeration/rate limits;
- authorization/ownership;
- CSRF;
- SQL binding;
- output encoding;
- uploads;
- password reset;
- webhooks;
- secrets/logging;
- destructive GET routes;
- admin permissions.

## Commerce integrity audit

Trace an example product through:

```text
catalog price/stock
 -> PLP
 -> PDP
 -> cart
 -> checkout
 -> order item snapshot
 -> admin order
 -> refund/return if applicable
```

Look for divergent calculations or stale representations.

## Accessibility audit

At minimum:

- keyboard core journey;
- focus visibility/obscuring;
- dialog/filter focus;
- form labels/errors;
- dynamic status announcements;
- zoom/reflow;
- touch target issues;
- semantics/headings;
- contrast spot checks;
- authentication accessibility.

Automated scanners are useful but cannot replace manual interaction testing.

## Performance audit

Measure representative page types:

- Home;
- PLP/search;
- PDP;
- Cart;
- Checkout;
- Account;
- Admin heavy list.

Inspect:

- LCP resource;
- CLS sources;
- INP interaction hotspots;
- JS/third parties;
- image sizes;
- DB/query latency;
- unbounded/N+1 queries.

## Audit deliverable structure

```text
Executive summary
Scope and environment
Critical/high findings
Journey findings
Security findings
Commerce integrity findings
Accessibility findings
Performance findings
Admin/operations findings
Analytics/SEO findings
Prioritized remediation plan
Verification checklist
Unverified areas
```

Use `templates/audit-report.md`.

## Audit rule

Never write "secure", "accessible", or "production ready" when only static code review was performed. State exactly what was and was not tested.
