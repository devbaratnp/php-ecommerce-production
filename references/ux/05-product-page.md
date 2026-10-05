# Product Detail Page (PDP)

## PDP job

Help a customer decide whether the specific product/variant is right, understand the full purchase implications, and complete the next action confidently.

NN/g and Baymard both treat PDPs as a large independent UX domain because product evaluation requires more than title/price/add-to-cart.

## Recommended information architecture

```text
Breadcrumbs
Product identity
Product media
Buy box
  - name/brand
  - rating summary if real
  - price
  - variant selectors
  - stock
  - quantity
  - Add to cart / Buy action
  - delivery estimate/cost
  - returns summary
Key highlights
Description
Specifications/details
Size/fit/care as relevant
Delivery & returns detail
Reviews
FAQ/Q&A
Related/alternative/complementary products
Recently viewed (optional)
```

Order varies by vertical and viewport.

## Product identity

Clearly expose:

- product name;
- brand when useful;
- category/breadcrumb;
- SKU/model/identifier where customers use it;
- selected variant identity.

Do not hide model/SKU for electronics/B2B where customers compare exact items.

## Buy box

The buy box should keep the key purchase decision close together:

- current price;
- legitimate sale/reference price;
- availability;
- required variants;
- quantity;
- delivery information;
- purchase action;
- concise return/support reassurance where relevant.

Do not scatter required choices across distant page sections.

## Pricing

If variants differ:

- update displayed price when selection changes;
- if no variant selected and range exists, show an accurate range or "from" price;
- do not show a low starting price while preselecting a much more expensive variant without clarity.

## Availability

Use meaningful states:

- In stock;
- Only X left only when genuine and business chooses to expose count;
- Out of stock;
- Preorder/backorder with expected timing;
- Discontinued/unavailable.

Avoid red urgency styling for ordinary availability without reason.

## Delivery and total-cost context

Near the buy section, expose enough to answer:

- Can it be delivered to me?
- What does delivery likely cost?
- When will it arrive?
- Is there a free-delivery threshold?

Research basis: Baymard product-page testing finds customers seek shipping/total-cost information before committing to cart.

Pattern:

```text
Deliver to: Kathmandu  [Change]
Rs 150 delivery
Estimated Sep 24–26
Free delivery over Rs 5,000
```

If calculation requires location, allow location selection progressively.

## Returns

Show a concise summary near purchase when return risk matters:

```text
7-day returns on unused items   [Details]
```

Do not use generic "Easy returns" without policy detail somewhere accessible.

## Product content layout

Core PDP content should be reachable through natural vertical scrolling.

Research basis: Baymard's updated testing reports horizontal tab navigation for primary product sections performs poorly because users may not realize important content is hidden.

Preferred:

- expanded sections; or
- vertical accordions/collapsible sections.

Use horizontal tabs only for secondary contexts where discoverability risk is acceptable and keyboard/accessibility behavior is excellent.

## Description

Write for decision support:

- what the product is;
- who/what it is for;
- key benefits/capabilities;
- constraints/compatibility;
- contents/materials as relevant.

Do not repeat SEO keywords unnaturally.

## Specifications

Use structured label/value tables for comparable technical information.

Example:

```text
Capacity      256 GB
Display       6.7 in
Warranty      1 year
Model         ABC-123
```

For long tables, group logically.

## Size/fit

For apparel:

- variant sizing visible;
- size guide near selector;
- measurement units clear;
- fit/model information where available;
- do not force customers to leave the page to understand size.

## Related product strategies

Separate intent:

- **alternatives**: similar substitute products;
- **complements**: accessories/works-with;
- **recently viewed**: context recovery;
- **frequently bought together** only if based on real data/merchandising.

Do not label random adjacent database rows "Recommended for you."

## Sticky mobile purchase action

May help long PDPs when:

- selected variant/price is clear;
- it does not obscure content/focus;
- required options cannot be bypassed;
- bar remains compact.

If options are incomplete, CTA should guide to the missing choice rather than silently adding a default.

## PDP acceptance checks

- [ ] Product image/title/price/options all describe the same product state.
- [ ] Required variants are obvious before purchase.
- [ ] Price/stock update consistently with variant.
- [ ] Delivery/cost information is discoverable near buying decision.
- [ ] Returns/support information is accessible.
- [ ] Main product content is reachable by vertical scrolling.
- [ ] Technical specs use structured labels where relevant.
- [ ] Add-to-cart has clear feedback and error recovery.
- [ ] Related products are relevant by defined logic.
- [ ] Mobile purchase action does not cover content/focus.
