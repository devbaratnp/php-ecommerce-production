# Pricing, Discounts, and Promotions

## Principle

Price is a server-side business fact, not a hidden form value.

The same pricing engine should feed:

- PDP display;
- product card display where possible;
- cart;
- checkout;
- order creation;
- admin preview/recalculation tools.

Do not independently reimplement discount logic in JavaScript and PHP.

## Money representation

Use `DECIMAL` in SQL or another exact monetary representation appropriate to the system. Centralize rounding rules.

All monetary calculations should carry explicit currency context.

## Price concepts

Distinguish:

- regular/base price;
- sale price;
- customer-group price;
- variant price;
- quantity/tier price;
- coupon/promotion discount;
- shipping;
- tax;
- fee;
- grand total.

Avoid an ambiguous single `price` field if the business needs multiple concepts.

## Price display

Rules:

- display currency consistently;
- show compare-at price only when a real valid discount exists;
- never fabricate a crossed-out reference price;
- sale badges must derive from the same price state as checkout;
- range price ("From Rs X") must be accurate for products with variant price differences;
- if price depends on quantity/customer group, explain it near the decision.

## Cart repricing

A cart can retain a display snapshot, but on every meaningful cart/checkout render the server should be able to reprice current lines.

If price changed since add-to-cart:

- update to authoritative price according to business policy;
- communicate material change clearly;
- do not silently charge a higher amount than the customer saw without surfacing it before placement.

## Promotion model

A promotion needs eligibility rules and benefit rules.

Eligibility examples:

- active date/time window;
- minimum cart value;
- category/product inclusion/exclusion;
- customer group;
- first order;
- usage count;
- per-customer limit;
- payment method;
- location.

Benefit examples:

- fixed amount;
- percentage;
- free shipping;
- buy X get Y;
- bundle/tier price.

## Coupon validation lifecycle

Validate when applied and again at checkout.

Possible invalidation reasons:

- expired;
- not started;
- usage exhausted;
- customer not eligible;
- cart no longer meets minimum;
- conflicting promotion;
- product no longer eligible.

Return a customer-readable reason without revealing sensitive internal rules.

## Promotion stacking

Define explicitly:

- can coupons stack with automatic promotions?
- can two coupons stack?
- which discount applies first?
- which line receives discount allocation?
- what happens on partial return/refund?

If the answer is not defined, refund accounting will become inconsistent later.

## Discount allocation

For line-level refund/reconciliation, allocate order-level discounts to order items using a deterministic rule.

Document rounding behavior so allocated line discounts sum exactly to order discount total.

## Shipping threshold

If free shipping is based on cart amount, define whether threshold considers:

- subtotal before discounts;
- subtotal after discounts;
- tax;
- excluded heavy items.

The customer-facing message and server rule must match.

## Tax

Tax rules vary by jurisdiction and business. This skill does not prescribe tax law.

Engineering rule:

- centralize tax calculation;
- record the tax result/snapshot on the order;
- never infer legal tax treatment from a generic template;
- make checkout totals explain tax according to local requirements.

## Fraud/deceptive pricing guardrails

DO NOT:

- inflate compare-at price to manufacture a discount;
- show countdown-based sale expiry when it automatically resets deceptively;
- claim "lowest price" without a verifiable rule;
- apply hidden mandatory fees only at the final action.

## Pricing acceptance tests

- [ ] Product/PDP/cart/checkout prices agree for the same state.
- [ ] Browser-tampered price is ignored.
- [ ] Coupon revalidates at checkout.
- [ ] Expired/ineligible discounts cannot create orders.
- [ ] Stacking rules are deterministic.
- [ ] Rounding totals reconcile exactly.
- [ ] Sale/compare-at copy reflects real server data.
- [ ] Refund calculations account for allocated discounts.
