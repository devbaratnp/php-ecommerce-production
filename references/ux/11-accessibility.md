# Accessibility

## Target

Use **WCAG 2.2 AA** as the baseline for core storefront, checkout, account, and admin journeys unless the project has a stricter applicable requirement.

This file is an engineering checklist, not a legal compliance opinion.

## Native semantics first

Prefer:

- `<button>` for actions;
- `<a href>` for navigation;
- native form fields;
- `<dialog>` or well-tested accessible modal pattern;
- semantic headings/landmarks/tables where appropriate.

Do not create clickable `<div>` controls and then attempt to recreate every keyboard/accessibility behavior manually.

## Page structure

MUST include:

- useful document title;
- one logical page `<h1>` in ordinary page structures;
- hierarchical headings;
- `<header>`, `<nav>`, `<main>`, `<footer>` landmarks;
- skip link to main content;
- meaningful link/button text.

## Keyboard

All core functionality must be keyboard operable.

Test:

- header navigation;
- autocomplete;
- filters;
- variant selectors;
- gallery controls;
- cart quantity/remove;
- checkout;
- modals/sheets;
- account/order actions;
- admin tables/actions.

No keyboard traps except intentional modal focus containment with a clear exit.

## Focus visibility

Keep a clearly visible focus indicator.

WCAG 2.2 includes Focus Not Obscured at AA: sticky headers/footers/dialogs cannot completely hide the focused control.

Project standard:

- use `scroll-padding`/layout offsets;
- test fixed headers, bottom nav, sticky buy bars, cookie banners together;
- restore focus to opener after modal closes.

## Target size

WCAG 2.2 AA includes 24x24 CSS px minimum target-size criteria with exceptions.

For ecommerce touch usability this skill uses a stronger project target of approximately 44x44 CSS px for primary standalone controls where practical.

## Color/contrast

Meet WCAG contrast requirements for:

- body text;
- small price/metadata;
- links;
- focus indicators;
- badges/status text;
- disabled states still needing readability;
- placeholders (while not used as labels).

Do not communicate stock/error/sale state through color alone.

## Images

Product image `alt` should convey useful identity/information without duplicating nearby text excessively.

Decorative images use empty alt.

Gallery thumbnails/control labels must distinguish images sufficiently.

## Forms

Every input needs a programmatic label.

Errors:

- identify field;
- explain correction;
- connect using `aria-describedby` or equivalent;
- do not disappear before user can read;
- preserve valid data.

Do not rely on red border alone.

## Authentication

WCAG 2.2 Accessible Authentication emphasizes avoiding cognitive-function barriers unless an alternative/mechanism exists.

Practical requirements:

- allow password managers;
- allow paste into password/OTP fields unless a provider constraint is justified;
- use correct autocomplete tokens;
- do not require users to manually transcribe complicated codes when a supported automatic mechanism exists;
- CAPTCHA must have accessible alternatives if used.

## Status messages

Dynamic feedback that does not take focus still needs programmatic announcement when important.

Examples:

- "Added to cart";
- cart count changed;
- filter applied;
- form saved;
- stock became unavailable.

Use appropriate live region/status semantics. Avoid announcing every minor keystroke/change noisily.

## Dialogs/sheets

When modal:

- accessible name/title;
- focus enters appropriately;
- focus stays within while open;
- Escape closes unless unsafe for the operation;
- close button available;
- focus returns to opener;
- background not interactable for assistive technologies.

## Autocomplete

Custom search autocomplete should follow a proven ARIA combobox/listbox pattern if implemented as an interactive popup.

Keyboard selection, active descendant/state, and screen-reader feedback must be tested rather than guessed.

## Variants/swatches

A color swatch needs text/accessible name, e.g. "Navy".

Selected/unavailable state must be programmatically determinable.

Do not rely on a diagonal slash image alone to communicate unavailable size.

## Prices

Sale price UI must remain understandable to screen readers.

Avoid reading meaningless sequences such as:

> Rs 500 Rs 900 44 percent

without labels/context.

## Tables

Use real table semantics for data tables, with headers/scopes. On mobile, if converted visually to cards, preserve programmatic associations or render a semantic alternative appropriately.

## Motion

Honor `prefers-reduced-motion`.

Avoid unnecessary parallax, auto-rotating content, layout-shifting animations, or motion that blocks reading/purchasing.

## Zoom/reflow

Test text resizing to 200% separately from reflow. For vertically scrolling content, test at an equivalent viewport width of 320 CSS px (for example, a 1280 CSS px viewport at 400% browser zoom). For horizontally scrolling content, test at an equivalent height of 256 CSS px. Do not disable browser zoom with viewport settings.

Content and actions must remain usable without two-dimensional scrolling except for particular content that genuinely requires two-dimensional layout for use or meaning. An excepted table or diagram does not exempt surrounding text and controls. See [W3C Reflow guidance](https://www.w3.org/WAI/WCAG22/Understanding/reflow.html).

## Accessibility acceptance checks

- [ ] Keyboard-only core purchase journey succeeds.
- [ ] Skip link/landmarks/headings are logical.
- [ ] Focus is visible and not obscured.
- [ ] Forms have labels and linked errors.
- [ ] Cart/status changes are announced appropriately.
- [ ] Dialog/filter sheet manages focus correctly.
- [ ] Variant color/selected/unavailable states are nonvisual too.
- [ ] Touch controls meet required/project target sizing.
- [ ] Text resizing to 200% remains usable.
- [ ] Reflow at equivalent 320 CSS px width (or 256 CSS px height for horizontal content) preserves information and functionality, with exceptions limited to genuinely two-dimensional content.
- [ ] Reduced-motion preference is respected.
- [ ] Password manager/paste/autofill remain usable.
