# Analytics and Observability

## Two different systems

### Analytics

Answers customer/product questions:

- Are products discovered?
- Where does funnel drop?
- Which searches fail?

### Observability

Answers operational/technical questions:

- Why did checkout fail?
- Did a payment webhook arrive?
- Which request changed an order?

Do not confuse the two.

## Ecommerce event vocabulary

If using GA4 or mapping to another analytics system, consider standard ecommerce events such as:

```text
view_item_list
select_item
view_item
add_to_cart
remove_from_cart
view_cart
begin_checkout
add_shipping_info
add_payment_info
purchase
refund
```

Follow the analytics vendor's current parameter specification rather than inventing incompatible payloads.

## Custom UX events

Useful when they answer a decision:

```text
search
search_no_results
filter_apply
sort_change
wishlist_add
coupon_apply
coupon_error
checkout_validation_error
payment_failure
return_started
reorder
```

Do not track every click "just in case."

## Purchase event integrity

`purchase` should fire only for authoritative successful order/payment condition according to business model.

Prevent duplicate analytics purchase events on confirmation refresh. Use stable transaction/order identifier and analytics deduplication patterns.

Do not let analytics be the only record of revenue.

## Funnel definitions

Define exactly what each step means.

Example:

```text
Product view
 -> Add to cart
 -> Begin checkout
 -> Delivery selected
 -> Payment initiated
 -> Order placed
 -> Payment confirmed (if online)
```

COD/manual payment may require different interpretation than instant card capture.

## Search analytics

Aggregate:

- query volume;
- zero-result rate;
- result click-through;
- refinement/filter usage;
- purchase after search.

Use to improve synonyms/taxonomy/product data.

Do not expose sensitive search terms broadly in dashboards without privacy review.

## Client-side error telemetry

Track meaningful JS failures with:

- page/route;
- release version;
- browser/device class;
- stack/message;
- correlation/request ID when available.

Avoid logging form field values or PII unnecessarily.

## Server logging

Use structured logs where possible:

```text
timestamp
level
request_id
user_id (internal/minimized where appropriate)
route
action
entity/order/payment id
message
safe metadata
```

## Correlation IDs

Assign request ID per request and include it in:

- server logs;
- integration/payment logs;
- safe error response/support reference when useful.

This makes "customer says checkout failed" traceable.

## Payment observability

Record:

- local payment attempt;
- provider request/reference;
- redirect/callback;
- webhook event IDs;
- signature verification result (not secret);
- state transition;
- reconciliation mismatch.

Never log card data/secrets.

## Admin audit log

Separate business audit from technical logs.

Track:

- actor;
- sensitive action;
- entity;
- meaningful before/after;
- timestamp;
- reason for high-risk actions where required.

Audit records should not be casually editable by the same workflow they audit.

## Privacy

Minimize:

- personal data in analytics;
- raw addresses/phones in logs;
- session/token values;
- payment credentials;
- uploaded identity/private documents.

Honor consent/legal requirements applicable to deployment. This skill does not determine jurisdictional law.

## Performance RUM

Collect Core Web Vitals by page type/release where feasible:

- LCP;
- INP;
- CLS.

Segment by mobile/desktop and key page types.

## Alert candidates

- sudden 5xx increase;
- checkout error spike;
- payment webhook failure;
- order/payment mismatch;
- queue/email failure;
- abnormal inventory negative values;
- Core Web Vitals regression.

Small projects may use manual monitoring initially, but critical payment/order failures need a discoverable signal.

## Acceptance checks

- [ ] Purchase analytics cannot double-count on refresh easily.
- [ ] Search zero-results can be measured if search analytics enabled.
- [ ] Server exceptions include request correlation.
- [ ] Payment state transitions are traceable.
- [ ] Admin high-risk actions are auditable.
- [ ] Logs/analytics avoid prohibited/unnecessary sensitive data.
- [ ] Core Web Vitals field measurement exists when traffic/stack permits.
