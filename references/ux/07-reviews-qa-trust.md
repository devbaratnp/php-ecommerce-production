# Reviews, Q&A, Trust, and Reassurance

## Reviews are product-evaluation data

For stores with genuine customer reviews, treat reviews as structured content rather than a decorative star row.

Research basis: product-evaluation research from Baymard and NN/g consistently treats reviews as a major input to purchase decisions.

## Rating summary

Provide:

- average rating;
- review count;
- rating distribution when volume justifies it;
- clear link/jump to reviews.

Do not show an average without enough context when there are very few reviews.

## Review item

Useful fields:

- rating;
- title;
- review text;
- date;
- verified-purchase marker if truly verified;
- purchased variant where relevant;
- customer media;
- helpful vote;
- merchant response.

Respect privacy when choosing reviewer display names.

## Filtering/sorting reviews

For meaningful volume, consider:

- newest;
- highest/lowest;
- with photos/video;
- by product variant/size;
- verified purchase.

Do not offer filters with no underlying data.

## Review media

Customer images/video can reduce uncertainty.

If implemented:

- gallery/lightbox should allow moving through review media;
- tie media to source review;
- moderate unsafe/private content;
- optimize uploaded files;
- expose accessible controls.

## Merchant responses

Visually distinguish brand/merchant responses from customer reviews.

Responses should address the concern rather than attack/rebut the customer.

Do not edit a customer's review text to improve sentiment. If moderation removes content, define policy and audit moderation where needed.

## Review submission

Only customers/users allowed by business rules should submit.

Protect against:

- spam;
- CSRF;
- duplicate abuse;
- malicious uploads/XSS;
- rating manipulation.

Review form:

- rating;
- title/body as needed;
- optional media;
- clear moderation/publication expectations.

## Empty review state

If no reviews:

```text
No customer reviews yet.
[Write a review]  (if eligible)
```

Do not fabricate "5.0" to avoid an empty state.

## FAQ vs Q&A

These serve different roles.

### FAQ

Merchant-authored anticipated questions:

- warranty;
- compatibility;
- care;
- ingredients;
- sizing;
- shipping constraints.

### Product Q&A

Customer-originated questions/answers where implemented.

Q&A needs moderation, notification, ownership, and abuse controls. Do not add the UI unless operations can support it.

## Trust content

Trust should come from evidence:

- real contact/business identity;
- clear shipping and return policies;
- payment method explanation;
- warranty/support;
- provenance/certification with evidence;
- consistent product data;
- real reviews.

## Security badges

Do not invent "Secure Checkout" shields or third-party certifications.

If a real trust seal/certification exists:

- use according to provider rules;
- link/verify where appropriate;
- do not let a badge replace actual HTTPS/security practice.

## Scarcity/social proof

Only show:

```text
Only 2 left
25 purchased this week
```

when data is real, timely, and the business intentionally uses it.

Never simulate activity with random numbers.

## Returns and shipping trust paths

Expose both near the purchase decision and through explicit footer/help routes.

A generic `Help` link should not be the only way to discover critical shipping/return policy.

## Acceptance checks

- [ ] Ratings/reviews are backed by real records.
- [ ] Verified-purchase label is only applied after actual verification.
- [ ] Review media is moderated and safely stored.
- [ ] Merchant responses are clearly identified.
- [ ] FAQ/Q&A are separate concepts.
- [ ] No fake seals, scarcity, social proof, or review data.
- [ ] Shipping/returns/support are easy to locate.
- [ ] Review/Q&A UI has a real operational moderation path before launch.
