# Ecommerce Vertical Pattern Matrix

## Purpose

Do not apply the same storefront to every catalog. Activate patterns according to how customers evaluate and repurchase the products.

## 1. General single-vendor retail

Core priorities:

- clear taxonomy/search;
- product cards;
- PDP cost/delivery clarity;
- cart/guest checkout;
- account/order tracking;
- admin catalog/orders/inventory.

Optional by catalog:

- reviews;
- wishlist;
- compare;
- coupons.

## 2. Apparel / fashion

High-priority UX:

- color + size variants visible;
- size guide near selector;
- unavailable sizes visible;
- multiple imagery angles;
- on-person/model/context imagery;
- material/care/fit content;
- returns clarity;
- wishlist;
- review filtering by size/fit if data supports it.

PLP:

- variant swatches;
- secondary imagery;
- size/color/category/price filters.

Admin:

- variant matrix;
- SKU by size/color;
- bulk stock updates.

## 3. Grocery / FMCG / dairy

High-priority UX:

- pack size/weight visible;
- price per unit where useful;
- fast add/quantity changes from PLP;
- stock/substitution policy;
- delivery area/window;
- repeat/reorder;
- favorites;
- dietary/ingredient/allergen/nutrition data as applicable;
- expiry/perishability operations if relevant.

Avoid overly large editorial PDPs that slow repetitive basket building.

Admin:

- rapid stock update;
- batches/expiry only if business needs;
- substitution/out-of-stock management;
- delivery slots/zones where applicable.

## 4. Electronics / appliances

High-priority UX:

- model/SKU prominent;
- structured specifications;
- compatibility;
- comparison;
- warranty;
- included accessories;
- variant storage/color/model consistency;
- technical filters;
- availability/delivery.

PLP should expose comparison-relevant specs, not just image/name/price.

Admin:

- spec templates/category attributes;
- serial/warranty tracking if business requires.

## 5. Jewelry / luxury

High-priority UX:

- high-quality detail imagery;
- zoom;
- on-person/scale imagery;
- dimensions/weight/material/purity;
- certification/provenance evidence where applicable;
- care/warranty;
- insured/secure delivery explanation if relevant;
- return/resize/exchange policy;
- appointment/contact option if business uses assisted sales.

Never invent authenticity/certification badges.

Admin may need:

- unique piece/serial identifier;
- weight/purity fields;
- dynamic metal pricing only if explicitly required and carefully modeled.

## 6. B2B / wholesale / technical catalog

High-priority UX:

- SKU search;
- dense product tables;
- spec/compatibility filters;
- quantity breaks;
- MOQ/case pack;
- quote request;
- saved lists;
- quick order by SKU;
- reorder;
- company/customer-group pricing;
- PO/manual payment terms if applicable.

Account may need:

- company organization;
- buyers/roles;
- saved addresses;
- credit/payment terms;
- order approval.

Do not force image-heavy consumer cards where operators choose by part number/spec.

## 7. Food / prepared food

Potential priorities:

- availability hours;
- pickup/delivery zones;
- modifiers/add-ons;
- allergen information;
- preparation time;
- scheduled order;
- item availability by location/time.

Cart must preserve modifiers accurately.

## 8. Digital goods

Differences:

- no physical shipping fields;
- entitlement/download/access lifecycle;
- license/account association;
- instant fulfillment;
- refund policy;
- download security/expiry.

Do not show shipping estimate or address fields when irrelevant.

## 9. Hybrid physical store + online

Activate:

- store availability;
- pickup;
- store locator;
- inventory by location;
- pickup-ready notification;
- return-to-store policy.

Avoid claiming live store stock if inventory sync is delayed without communicating that limitation.

## Archetype selection template

```text
Primary vertical:
Secondary traits:
Catalog size:
Variants:
Repeat purchase frequency:
Evaluation style: visual / spec / habitual / assisted
Fulfillment:
Payment:
Required optional modules:
Excluded patterns:
```

## Rule

Vertical guidance changes priorities, not core integrity/security requirements.
