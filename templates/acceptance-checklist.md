# Ecommerce Acceptance Checklist

## Discovery and navigation

- [ ] New visitor can identify what the store sells.
- [ ] All categories/shop route is discoverable.
- [ ] Search works with Enter and without selecting autocomplete.
- [ ] Breadcrumb/current scope clear on deep pages.
- [ ] No dead navigation/hash links.

## PLP/search

- [ ] Result count/sort/filter functional.
- [ ] Filters backed by real catalog data.
- [ ] Applied filter summary visible.
- [ ] Filter/sort state preserved.
- [ ] Empty/error/no-results recovery exists.
- [ ] Product cards expose vertical-appropriate decision data.

## PDP

- [ ] Product/media/price/variant identity consistent.
- [ ] Required options visible and validated.
- [ ] Stock state accurate.
- [ ] Delivery cost/date discoverable.
- [ ] Returns/support accessible.
- [ ] Main content vertically discoverable.
- [ ] Add-to-cart provides feedback/error recovery.

## Cart

- [ ] Guest/auth add/update/remove works.
- [ ] Badge/cart state synchronized.
- [ ] Price/stock revalidated.
- [ ] Empty cart recovery path.
- [ ] Login cart merge safe.

## Checkout

- [ ] Guest checkout supported/prominent where applicable.
- [ ] Fields have labels/autofill/appropriate keyboard.
- [ ] Valid data preserved after errors.
- [ ] Delivery options show cost/date.
- [ ] Final total visible before final action.
- [ ] Server ignores tampered totals.
- [ ] Duplicate submit/idempotency protection.
- [ ] Confirmation based on persisted server state.

## Payment

- [ ] Online success verified server-side.
- [ ] Webhook signature verified/idempotent.
- [ ] Failure/retry path exists.
- [ ] COD/manual states accurate.
- [ ] Refund cannot exceed captured/paid amount.

## Account/self-service

- [ ] Order list identifiable without opening many records.
- [ ] Order detail shows payment + fulfillment accurately.
- [ ] Guest tracking secure if supported.
- [ ] Cancellation/return eligibility understandable.
- [ ] Reorder uses current price/availability.

## Admin

- [ ] Products/variants manageable.
- [ ] Inventory adjustments controlled/audited.
- [ ] Order queue actionable.
- [ ] Invalid status transitions blocked.
- [ ] Refund/payment verification controlled.
- [ ] Lists bounded/paginated.
- [ ] Permissions server-enforced.

## Security

- [ ] Secure session policy.
- [ ] Session regeneration after auth.
- [ ] CSRF.
- [ ] Prepared statements.
- [ ] Contextual output encoding.
- [ ] Upload validation/storage safety.
- [ ] Password reset safe.
- [ ] Rate limiting/abuse controls.
- [ ] Secrets not exposed.
- [ ] Production errors safe.

## Accessibility

- [ ] WCAG 2.2 AA target reviewed.
- [ ] Keyboard purchase journey passes.
- [ ] Focus not obscured.
- [ ] Dialog/filter focus correct.
- [ ] Form errors programmatic.
- [ ] Dynamic statuses announced.
- [ ] Zoom/reflow usable.
- [ ] Reduced motion supported.

## Performance

- [ ] Likely LCP resource optimized/not lazy-loaded.
- [ ] Responsive images.
- [ ] Layout dimensions reserved.
- [ ] No unbounded catalog/admin queries.
- [ ] No obvious N+1.
- [ ] Third parties audited.
- [ ] CWV lab/field status recorded.

## Analytics/SEO/deployment

- [ ] Purchase event deduplication if analytics enabled.
- [ ] Payment/order failures traceable.
- [ ] Canonicals/structured data/sitemap reviewed for public catalog.
- [ ] Production robots/noindex verified.
- [ ] HTTPS/debug/secrets/logs/backups verified.
