# Mobile and Responsive Ecommerce

## Principle

Responsive ecommerce is behavior adaptation, not only CSS shrinkage.

Baymard's 2026 mobile benchmarks still find most leading ecommerce mobile experiences only mediocre, reinforcing the need to test mobile-specific behaviors explicitly.

## Test viewports and conditions

At minimum test:

- narrow phone;
- common phone;
- large phone;
- tablet portrait/landscape where relevant;
- desktop;
- browser zoom/reflow;
- on-screen keyboard open;
- slow network / image delay;
- reduced motion.

Do not optimize only for one device screenshot.

## Mobile navigation

Customers must have an obvious route to:

- search;
- all categories/shop;
- cart;
- account/orders.

If bottom navigation exists:

- limit to primary destinations;
- label icons;
- show active state;
- keep badge readable;
- reserve content bottom padding.

## Search

Search should remain easy to activate without a tiny icon hidden behind several taps.

Autocomplete must fit above the keyboard and avoid an enormous suggestion list.

When keyboard opens:

- search submit remains usable;
- suggestion list scrolls independently if needed;
- no fixed bars cover selected suggestion.

## Product list

Mobile PLP priorities:

- clear result count/scope;
- prominent Filter and Sort;
- applied filter count/summary outside drawer;
- product cards readable without tiny text/actions;
- image loading tuned for viewport.

## Filter sheet/dialog

Requirements:

- accessible title;
- full-height or sensible sheet;
- close control;
- active values;
- clear/reset;
- apply if staged;
- focus trap for modal implementation;
- restore focus on close;
- body scroll handled correctly.

Do not clone desktop markup with duplicate IDs.

## Product card touch behavior

Hover-only interactions must have touch equivalents.

Never make price/variant/essential details appear only on hover.

Secondary-image swipe must not make vertical scrolling frustrating.

## PDP

Mobile PDP needs:

- clear image gallery index;
- thumb-friendly variant controls;
- size guide near sizes;
- sticky purchase action only if helpful and compact;
- delivery/returns close to buy decision;
- sections that work as vertical accordions where content is long.

## Sticky purchase bar

Test with:

- bottom navigation;
- cookie banner;
- chat/WhatsApp bubble;
- keyboard;
- error toast.

If everything is fixed, content can become unusable.

## Cart

Avoid desktop table squashed onto phone.

Use cards/stacked layout while preserving:

- identity;
- options;
- price;
- quantity;
- remove;
- totals.

Checkout CTA can be sticky only if it does not obscure cart editing/focus.

## Checkout

Mobile form rules:

- single-column fields by default;
- correct keyboard (`email`, `tel`, numeric as appropriate);
- no side-by-side tiny first/last name fields unless proven usable;
- tap targets generous;
- error remains visible when keyboard opens;
- payment SDK/iframe tested on actual mobile viewport;
- summary can collapse but total/final action remain understandable.

## Touch target project standard

WCAG 2.2 defines a 24x24 CSS px minimum with exceptions. This project standard aims higher for primary ecommerce touch controls:

**Target approximately 44x44 CSS px or greater where practical.**

This is a usability standard, not a claim that WCAG universally requires 44px.

## Gestures

Do not make drag/swipe the only way to perform an action if a simpler tap/control can provide equivalent functionality.

Examples:

- image gallery has buttons/dots accessible to non-drag input;
- sortable admin items need non-drag fallback if essential.

## Content density

Do not hide important product facts just to make mobile visually minimal.

Prioritize and collapse detail intelligently.

## Network constraints

For slower mobile networks:

- server render essential content;
- minimize JS bundles;
- responsive images;
- defer noncritical widgets;
- avoid huge libraries for minor animations;
- keep actions resilient after transient failure.

## Mobile acceptance checks

- [ ] Shop/all-categories route is discoverable.
- [ ] Search works with keyboard open.
- [ ] Applied filters are visible outside filter sheet.
- [ ] Hover-only behavior has touch equivalent.
- [ ] Sticky elements do not reduce usable viewport excessively.
- [ ] Checkout form uses appropriate mobile keyboards/autofill.
- [ ] Primary touch targets meet project sizing standard where practical.
- [ ] Galleries/filters/dialogs work with keyboard and touch.
- [ ] Slow image/API load does not destroy layout or core action.
