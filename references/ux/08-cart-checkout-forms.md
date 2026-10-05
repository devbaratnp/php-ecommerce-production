# Cart, Checkout, and Form UX

## Checkout objective

Move a customer from purchase intent to a correctly placed order with minimal uncertainty, unnecessary data entry, or dead ends.

Checkout UX must match the actual commerce/payment architecture. A polished multi-step UI cannot compensate for unreliable totals, stock, or payment state.

## Cart UX

A populated cart should expose:

- product image/name;
- selected variant/options;
- unit price;
- quantity control;
- line total;
- availability warning if state changed;
- remove;
- save for later only if implemented;
- subtotal;
- estimated shipping/tax/fees as applicable;
- checkout action.

The cart is not the place for aggressive cross-sell that pushes totals/actions below the fold on small screens.

## Empty cart

Show:

- clear empty state;
- continue shopping/category/search routes;
- optionally recently viewed/saved items when real.

Do not leave only a disabled checkout button.

## Mini-cart feedback

After Add to Cart, provide immediate confirmation through one or more:

- updated badge;
- inline status;
- mini-cart/drawer;
- toast with view-cart action.

Do not unexpectedly redirect to cart for every add unless the store intentionally uses that pattern.

## Guest checkout

Default supported path:

```text
[Continue as guest]  <- visually prominent
Sign in
Create account / account benefits
```

Research basis: Baymard's checkout research repeatedly finds forced or visually buried guest checkout creates friction.

Account creation can be offered after order confirmation when the user already supplied much of the required identity data.

## Checkout shell

A checkout can reduce distraction compared with the normal storefront header while still providing:

- trusted brand/home escape;
- progress/orientation;
- support/help route;
- cart/order summary.

Do not trap users with no safe way to review the cart.

## Step structure

For multi-step checkout, progress labels must match real steps, e.g.:

```text
1 Contact & Delivery
2 Payment
3 Review
```

Do not display 3 steps and secretly add extra full-page account/payment confirmation stages.

Small stores may use one-page checkout if it remains scannable and performant.

## Form field contract

Each field needs applicable:

- persistent visible label;
- optional/required indication;
- help/description only when useful;
- semantic input type;
- `autocomplete` token;
- appropriate `inputmode`;
- server validation;
- inline error linked programmatically;
- preserved valid value on error.

Placeholder is not a label.

## Collect only necessary information

Do not request:

- date of birth;
- gender;
- company;
- fax;
- second phone number;
- account password;

unless the business/fulfillment actually needs it.

Mark optional fields explicitly to reduce uncertainty.

## Name/address fields

Address structure differs by country. Keep field model configurable.

For Nepal-like projects, address may need:

- recipient name;
- phone;
- province;
- district;
- municipality/locality;
- ward/tole/street/landmark as business requires.

Do not force US-centric ZIP/state assumptions into every store.

When billing normally equals shipping, default to same and reveal separate billing only when requested/needed.

## Phone field

If phone is required, explain why when customers may question it:

```text
Phone number
Used by the courier if they need help delivering your order.
```

Use `type="tel"` and suitable autocomplete/inputmode; do not block valid international/local formatting unnecessarily.

## Browser autofill

Do not break native autofill with random field names, fake custom text inputs, or disabling autocomplete without a security reason.

Use stable semantic names/IDs and correct `autocomplete` values.

## Validation

Validation message should answer:

1. what is wrong;
2. what the customer should do.

Bad:

```text
Invalid input.
```

Better:

```text
Enter a valid 10-digit mobile number, e.g. 98XXXXXXXX.
```

Only use format examples that match local rules.

After failed submission:

- preserve valid inputs;
- show summary when many errors exist if helpful;
- focus/scroll to first invalid field without hiding it under sticky UI;
- connect error to field with accessible description.

## Apply buttons

Do not add an Apply button after ordinary selections unless the interaction is intentionally staged.

Examples that usually update automatically:

- delivery method;
- saved address;
- payment-method selection.

Promo/coupon code is a valid explicit Apply pattern.

## Coupon UX

Do not overemphasize coupon entry so customers leave to search for a code.

A compact/disclosed pattern is often appropriate:

```text
Have a promo code?
```

After apply, show:

- code;
- benefit;
- remove;
- failure reason when invalid.

## Delivery method

Each option should show enough context:

```text
Standard delivery
Rs 150
Expected Sep 24–26
```

rather than only "Standard".

## Payment selection

Expose real methods with clear labels/logos where useful.

When method changes, CTA should reflect action:

```text
COD -> Place order
Card -> Pay Rs 4,250
Wallet -> Continue to wallet
```

Do not preselect a surprising paid/financing method.

## Order summary

Before final action, show:

- products/quantities;
- delivery destination summary;
- delivery method/date;
- payment method;
- subtotal;
- discounts;
- shipping;
- tax/fees;
- grand total.

Editing should return to the relevant section without discarding unrelated valid data.

## Submission state

On final submit:

- disable duplicate interaction while request is in flight;
- communicate processing;
- server still enforces idempotency;
- do not permanently disable the only action if network request fails;
- recover safely from timeout/unknown payment state.

## Confirmation page

Show persisted facts:

- success/status;
- order number;
- item summary;
- total;
- delivery expectation;
- payment state;
- next steps;
- tracking/order route;
- support.

For guest orders, optionally offer account creation/linking after confirmation.

## Acceptance checks

- [ ] Guest checkout is visible and works where applicable.
- [ ] Forms use persistent labels and native semantics.
- [ ] Autofill/password managers are not unnecessarily blocked.
- [ ] Required/optional state is clear.
- [ ] Valid values survive errors.
- [ ] Delivery methods show cost + expectation.
- [ ] Final total is visible before final action.
- [ ] CTA wording matches selected payment method.
- [ ] Duplicate submit is protected in UI and server.
- [ ] Confirmation does not rely on browser-only payment claim.
