# Internationalization and Localization

## Principle

Do not hardcode one country's assumptions into domain models, forms, money, dates, or fulfillment unless the project is explicitly local-only and the decision is documented.

## Currency

Store currency alongside monetary records when multi-currency is possible.

Use ISO 4217-style currency codes internally where appropriate.

Formatting belongs to presentation/localization:

```text
NPR 1,500
Rs 1,500
$1,500.00
```

based on project/locale, not string concatenation across business logic.

## Money conversion

If multi-currency pricing exists, decide whether:

- prices are independently managed per currency; or
- calculated from base currency using exchange rate.

Do not change an existing order when exchange rates change.

## Dates/time

- store technical timestamps consistently (often UTC);
- display in customer/business locale;
- explicitly model store-local cutoffs/holidays;
- avoid ambiguous `03/04/2026` presentation.

## Address formats

Address fields/order differ by country.

Do not universally require:

- ZIP/postal code;
- US state;
- county;
- street number.

Make address schema/validation appropriate to shipping countries.

For Nepal projects, reusable location data may include province, district, municipality/local level, ward, tole/street/landmark based on fulfillment need.

## Phone numbers

Decide supported countries and normalization strategy.

Do not strip `+` or leading digits blindly.

Store normalized form if possible while preserving display/input expectations.

## Language

When multiple languages:

- localize navigation, forms, validation, checkout, transactional messages;
- avoid images containing essential text;
- keep translated product content associated clearly;
- support longer/shorter strings without broken layouts;
- define fallback language.

## Units

Products may use:

- kg/g;
- lb/oz;
- cm/in;
- ml/L;
- pieces/packs.

Store normalized measurable values when calculations depend on units. Display according to market/product conventions.

## Taxes/duties

Legal rules are jurisdiction-specific. Do not infer them from this skill.

UX should clearly distinguish where applicable:

- tax included/excluded;
- import duties;
- collection timing.

## Shipping availability

Country/region selector should influence:

- address validation;
- delivery method;
- shipping cost;
- product restrictions;
- currency if the store uses regional pricing.

Do not let customer complete checkout to an unsupported destination and fail only after payment.

## SEO/localized URLs

If localized public pages exist, use a consistent URL and language strategy and appropriate search-engine signals. See `references/quality/05-seo-discoverability.md` and current Google documentation.

## Acceptance checks

- [ ] Money is not represented as locale-formatted strings in business logic.
- [ ] Historical order currency/amount remains immutable.
- [ ] Address form matches supported country requirements.
- [ ] Date format is unambiguous to target users.
- [ ] Phone normalization does not corrupt valid supported numbers.
- [ ] Translation does not hide critical validation/actions.
- [ ] Unsupported shipping regions are rejected before payment.
