# Ecommerce UX Principles

## Purpose

These principles govern all customer-facing modules. They are not visual style rules. They are behavior rules designed to help customers find, evaluate, purchase, and manage products without unnecessary uncertainty or effort.

## 1. Design around customer questions

Each stage should answer a different question.

### Homepage / category navigation

> What does this store sell and where should I begin?

### Search / PLP

> Which products match what I need?

### Product page

> Is this specific product right for me, and what will it really cost/get me?

### Cart

> Is my intended order correct, and what is the likely total?

### Checkout

> What information do I need to provide, what will I pay, and what happens next?

### Confirmation

> Did the order succeed and what should I expect now?

### Account/self-service

> Where is my order and what can I do about it?

Do not let decoration consume attention needed to answer these questions.

## 2. One visible control = one real behavior

Every visible interactive affordance MUST:

1. work;
2. clearly show it is unavailable and explain why when useful; or
3. be removed.

Examples of forbidden fake UI:

- wishlist heart with no storage/action;
- compare checkbox with no comparison page;
- filter options not backed by catalog/query logic;
- review stars that are static decoration;
- fake "12 people viewing" counters;
- "Only 2 left" when no stock source supports it;
- countdown timers that reset deceptively;
- payment logos for unsupported methods.

## 3. Preserve customer context

Returning from a PDP should preserve the customer's browsing context where possible:

- search query;
- category;
- filters;
- sort;
- page/load position;
- selected location where safe.

Validation failure in checkout should preserve all still-valid entered data.

Session expiry should not discard an entire cart or non-sensitive checkout context without necessity.

## 4. Reveal cost before commitment

Customers should not discover mandatory cost only after investing heavily in checkout.

Where relevant expose:

- product price;
- sale/reference price if legitimate;
- shipping estimate or threshold;
- tax/fees according to local model;
- payment-method fee if applicable/lawful;
- final total before the final order/payment action.

Research basis: cost transparency and shipping information are recurring product-page/checkout concerns in Baymard research.

## 5. Make delivery understandable

Use an expected arrival date/range when the system can estimate it honestly.

Prefer:

```text
Expected Sep 24–26
```

over only:

```text
3–5 business days
```

because customers ultimately need to know when they receive the item.

## 6. Make uncertainty explicit

Do not pretend precision that the system does not possess.

Good:

- "Estimated delivery Sep 24–26"
- "Stock confirmation happens at checkout"
- "Payment awaiting verification"

Bad:

- "Guaranteed tomorrow" without a guarantee;
- "Paid" before gateway verification;
- "In stock" from stale UI data while checkout uses another source.

## 7. Progressive disclosure, not information hiding

Show the information needed for the current decision prominently. Deeper details can be progressively disclosed.

Examples:

- PDP shows price, stock, delivery, returns summary near buy section; detailed policy lower down;
- checkout shows essential fields first; optional second address line can be disclosed;
- filters show common values first with "Show more" for long sets.

Do not hide essential content behind horizontal tabs that users may never inspect. For core PDP sections, expanded or vertical accordion patterns are generally safer.

## 8. Mobile is not compressed desktop

Mobile requires explicit decisions about:

- all-category access;
- search prominence;
- filter/sort sheet behavior;
- image swipe/zoom;
- sticky purchase actions;
- bottom navigation;
- keyboard/input modes;
- viewport overlap;
- touch target size;
- reduced density.

See `10-mobile-responsive.md`.

## 9. Accessibility is interaction quality

Use semantic HTML and native controls first.

If a control cannot be used reliably by keyboard, screen reader, zoom, touch, or alternative input, it is incomplete.

Critical changes such as "Added to cart" must not be communicated only through color/animation.

See `11-accessibility.md`.

## 10. Performance changes behavior

Slow product imagery, delayed filters, laggy add-to-cart, and layout shifts are UX defects.

Treat Core Web Vitals and perceived responsiveness as part of component acceptance.

## 11. Default to customer control

Avoid:

- forced registration;
- auto-applying surprising substitutions;
- unwanted newsletter opt-in;
- preselected paid add-ons;
- hidden recurring subscription choices;
- difficult-to-cancel flows;
- modal chains that block browsing.

Important choices should be explicit and reversible where practical.

## 12. Trust comes from evidence and consistency

Trust is built through:

- real business identity/contact information;
- consistent prices/statuses;
- clear delivery/return/payment information;
- real reviews;
- reliable error handling;
- professional, stable UI;
- no deceptive scarcity/social proof;
- predictable support and self-service.

Do not "add trust" by decorating checkout with invented security badges.

## 13. Build all meaningful states

Every component should consider applicable states:

- default;
- loading;
- success;
- empty;
- error;
- unavailable;
- disabled;
- partial data;
- session expired;
- offline/network failure where relevant.

See `13-states-feedback.md`.

## 14. Use research as a constraint, not a paint-by-numbers template

Research identifies recurring problems and patterns. The store archetype, catalog, customer, fulfillment, market, and technical constraints decide the exact implementation.

For example:

- a fashion store benefits heavily from size/color imagery and returns clarity;
- B2B industrial catalog may need dense product tables and SKU/spec filters;
- grocery needs rapid repeat quantity/reorder and substitution logic;
- jewelry needs detail/scale imagery and trust/provenance.

Use `references/optional/03-vertical-patterns.md`.
