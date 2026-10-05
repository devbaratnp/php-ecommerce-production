# Ecommerce Content and Microcopy

## Principle

Microcopy reduces uncertainty at decision points. It should explain consequences, not decorate the interface.

## Button labels

Prefer action-specific labels:

```text
Add to cart
Buy now
Continue to payment
Place order
Pay Rs 4,250
Track order
Start return
Save address
```

Avoid generic:

```text
Submit
Click here
Proceed
OK
```

when the actual action can be named.

## Availability copy

Use truthful operational language:

```text
In stock
Out of stock
Preorder — expected ...
Available for delivery
Pickup unavailable at this location
```

Avoid artificial urgency.

## Price copy

When showing sale/reference price, labels should make meaning clear if visual styling alone is ambiguous.

Examples:

```text
Now Rs 1,499
Was Rs 1,999
Save Rs 500
```

Only if the comparison is legitimate.

## Delivery copy

Customer-centered:

```text
Arrives Sep 24–26
Free delivery
Rs 150 delivery
Order by 2 PM for same-day delivery
```

Only show cutoff/countdown when it is real and fulfillment can honor it.

## Return copy

Concise summary near buy decision:

```text
7-day returns on unused items
```

with direct policy details accessible.

Avoid "Hassle-free returns" as the only information.

## Form helper copy

Explain why a non-obvious field is required:

```text
Phone number
Used by the courier if they need delivery help.
```

Do not add helper text under every obvious field; excessive text increases scanning burden.

## Error copy pattern

```text
Problem + correction
```

Examples:

```text
This email address is incomplete. Enter an address like name@example.com.
Only 3 units are available. Reduce the quantity to continue.
Your payment wasn't completed. No new charge was confirmed. Try again or choose another method.
```

Do not state "no charge" unless payment architecture can prove it.

## Empty state copy

Explain state + recovery:

```text
Your cart is empty.
Browse Dairy Products
```

```text
No products match all selected filters.
Clear filters or remove one option.
```

## Account/auth copy

Differentiate:

```text
Sign in
Create account
Continue as guest
```

Do not relabel account creation as "Continue" if it surprises the user.

## Confirmation copy

State exactly what happened:

```text
Order placed
Order #NP-48291
We've sent the confirmation to ...
```

For manual payment:

```text
Order received — payment awaiting verification
```

not "Payment successful."

## Trust claims

Claims such as:

- organic;
- certified;
- official;
- guaranteed;
- authentic;
- best price;

need evidence/process behind them.

Do not let marketing copy outrun operational reality.

## Localization

Do not concatenate sentence fragments around variables in a way that cannot be translated naturally.

Keep:

- dates;
- currency;
- plurals;
- address terminology;
- units;
- local payment names;

locale-aware.

## Tone

Prefer clear, concise, calm, customer-centered language.

Avoid blaming user:

Bad: "You entered an invalid address."

Better: "We couldn't verify this address. Check the ward/street details or continue with the address as entered" when the system supports that choice.

## Microcopy acceptance checks

- [ ] Primary CTAs state the actual action.
- [ ] Payment/confirmation copy matches real state.
- [ ] Non-obvious required fields explain why.
- [ ] Error copy includes a recovery action.
- [ ] Empty states offer a useful route.
- [ ] Scarcity/quality/trust claims are evidence-backed.
- [ ] Dates/currency/units are locale-aware.
