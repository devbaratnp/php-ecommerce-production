# Design System and Ecommerce Components

## Principle

When an ecommerce project already has an approved visual system, implementation should preserve it. Do not "improve" the design by importing a new dashboard/storefront aesthetic unless redesign is explicitly requested.

Before creating components, inspect:

- typography;
- spacing scale;
- colors/tokens;
- border/radius patterns;
- shadows/elevation;
- button/input styles;
- icon system;
- cards;
- breakpoints;
- motion;
- existing states.

Document the system rather than duplicating near-identical values page by page.

## Token layers

A simple token hierarchy can include:

```text
foundation
- spacing
- type scale
- radius
- elevation
- breakpoints

semantic
- surface
- text
- border
- primary action
- danger
- warning
- success
- focus

component
- button heights
- input height
- card padding
- header height
```

Do not hardcode one project's brand colors into the reusable skill.

## Required ecommerce component inventory

Most projects benefit from shared components for:

### Global

- Header
- Mobile navigation
- Footer
- Breadcrumbs
- Pagination
- Alert/flash
- Empty state
- Error/retry block
- Dialog/sheet
- Loading indicator/skeleton

### Catalog

- Product card
- Money/price display
- Sale/reference price
- Stock/availability label
- Rating summary
- Variant swatch
- Filter group
- Applied-filter chip
- Sort control

### PDP

- Product gallery
- Variant group
- Quantity stepper
- Delivery estimate
- Returns summary
- Add-to-cart action
- Review summary/item
- Specification list/table

### Checkout

- Form field
- Address block
- Delivery option
- Payment option
- Order summary
- Coupon input
- Checkout progress

### Account

- Order card
- Status timeline
- Tracking block
- Return eligibility/action

### Admin

- Admin shell/sidebar
- Stat/action card
- Data list/table
- Search/filter bar
- Status badge
- Confirmation dialog
- Audit timeline

## Component behavior first

A shared component is more than a CSS partial.

For every component define:

- inputs/data shape;
- variants;
- interactive behavior;
- disabled/unavailable state;
- validation/error state;
- mobile behavior;
- accessibility semantics;
- loading behavior.

Example:

```text
ProductCard
Inputs:
- product identity
- URL
- primary/secondary image
- price state
- stock state
- variant preview
- rating state

States:
- normal
- sale
- out of stock
- missing image
- loading image
```

## Button hierarchy

Use a small stable hierarchy:

- primary: main next action;
- secondary: alternative action;
- tertiary/link: low-emphasis action;
- destructive: irreversible/high-risk.

Do not place several visually identical "primary" actions next to each other without hierarchy.

## Form component

A reusable field should support:

```text
label
required/optional hint
help text
input/control
success (rarely necessary)
error
```

It must generate unique IDs and label/error associations.

Do not duplicate desktop/mobile versions with the same `id` values.

## Status color system

Color mapping should be semantic and consistent, but labels/icons/text remain necessary.

Example dimensions:

- neutral/pending;
- info/in-progress;
- success/completed;
- warning/attention;
- danger/failed/cancelled.

Do not use the same "green" meaning for Paid and Delivered if operators need to distinguish payment vs fulfillment. The text label carries the business state.

## Responsive component contracts

Each shared component documents breakpoint changes.

Example DataList:

```text
Desktop: semantic table
Tablet: reduced columns + details
Mobile: stacked cards with primary fields/actions
```

Avoid hiding essential data on mobile without another accessible path.

## Motion

Default ecommerce motion should explain state, not entertain.

Good:

- sheet entering;
- cart badge update;
- accordion expansion;
- image transition.

Avoid:

- every card animating on scroll;
- repeated floating/bouncing CTA;
- layout-based animation causing CLS;
- motion required to understand state.

Honor `prefers-reduced-motion`.

## Icons

Icons need consistent source/style.

- icon-only buttons require accessible name;
- do not use obscure icons without label for primary commerce actions;
- don't load multiple icon libraries unnecessarily;
- SVGs should not contain untrusted/scripted content from uploads.

## Loading components

Avoid generic skeleton everywhere.

Use when content delay is expected and skeleton geometry matches actual layout. For very fast server-rendered responses, a skeleton can create more visual churn than benefit.

## Existing UI implementation workflow

When static screens are provided:

1. inventory repeated patterns;
2. identify existing tokens;
3. build shared components without visual redesign;
4. replace duplicated screen markup gradually;
5. wire dynamic data/state;
6. compare screenshots across breakpoints;
7. test interactive/accessibility states.

Do not start by replacing all HTML with a new framework/component kit.

## Design-system acceptance checks

- [ ] Existing approved visual language is preserved unless redesign requested.
- [ ] Repeated components share one implementation.
- [ ] Components define loading/error/disabled states.
- [ ] IDs/labels remain unique when components repeat.
- [ ] Responsive behavior documented for complex components.
- [ ] Primary action hierarchy is clear.
- [ ] Motion respects reduced-motion and avoids layout shifts.
- [ ] No multiple icon/UI libraries without reason.
