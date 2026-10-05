# Example Invocations

## 1. Build a new store

```text
Use the php-ecommerce-production skill.

Build a single-vendor dairy ecommerce site in plain PHP 8 + MySQL. First classify the store archetype, create a feature map and architecture/data plan, then implement. Guest checkout, COD/manual QR, product variants by pack size, delivery areas, customer order tracking, and mobile admin are required. Do not code before the discovery/feature-map pass. Run the release gates before calling it complete.
```

Expected references:

- architecture;
- security;
- catalog/inventory/pricing/cart/checkout;
- grocery/FMCG vertical;
- storefront UX;
- admin;
- release tests.

## 2. Approved UI -> dynamic ecommerce

```text
Use IMPLEMENT mode from php-ecommerce-production.

The static HTML/CSS/JS screens are approved. Audit all routes and interactive controls first. Build shared components and map every UI control to a real server behavior. Keep the visual design unless a behavior/accessibility defect requires a minimal change. Use PDO/service-layer logic and make catalog, auth, cart, guest checkout, orders, payment state, and admin screens functional. Remove dead controls rather than leaving fake behavior.
```

## 3. Audit an existing PHP store

```text
Use AUDIT mode. Do not modify code yet.

Audit this PHP ecommerce repo for security, commerce integrity, storefront UX, admin operations, accessibility, performance, and release readiness. Reproduce critical journeys where the environment permits. Every finding must include evidence, severity, root cause, fix, and verification. Mark untested areas UNVERIFIED.
```

## 4. Fix duplicate orders

```text
Use FIX mode. Trace the complete checkout request and payment flow before editing. Reproduce double-click/refresh/retry behavior, identify why duplicate orders are created, and implement server-side idempotency plus PRG/state lookup as appropriate. Add a regression scenario from the journey-test reference.
```

## 5. Improve product page UX

```text
Use UX IMPROVEMENT mode.

Audit the current PDP against the Product Page, Product Media & Variants, Trust/Reviews, Mobile, Accessibility, and Performance references. Do not redesign for fashion trends. Preserve the site's visual system and propose only changes that improve product evaluation, price/delivery/return clarity, variant selection, gallery behavior, and purchase feedback.
```

## 6. Release review

```text
Use RELEASE mode. Run every applicable production release gate. Do not treat implementation presence as verification. Report PASS / FAIL / UNVERIFIED / NOT APPLICABLE with evidence and blockers. The final result must explicitly list known limitations.
```
