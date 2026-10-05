# Research and Source Registry

This package is a synthesis. The rules are not copied wholesale from any one source. Public research and standards were used to identify recurring user problems, platform constraints, and measurable baselines. Engineering decisions marked as "Project standard" are deliberate implementation choices layered on top of that evidence.

Last research refresh: **2026-09-21**.

## Ecommerce UX research

### Baymard Institute

Public research and benchmark overviews used during the v2 redesign:

- Homepage & Category Navigation UX: https://baymard.com/research/homepage-and-category-usability
- Ecommerce Search UX: https://baymard.com/research/ecommerce-search
- Product Lists & Filtering UX: https://baymard.com/research/ecommerce-product-lists
- Cart & Checkout UX: https://baymard.com/research/checkout-usability
- Checkout UX Best Practices / current state: https://baymard.com/research-articles/current-state-of-checkout-ux
- Checkout flow optimization guide: https://baymard.com/learn/checkout-flow-ux-optimization
- Product Page UX current state: https://baymard.com/research-articles/current-state-ecommerce-product-page-ux
- Avoid horizontal tabs for primary PDP sections: https://baymard.com/research-articles/avoid-horizontal-tabs
- Product-page shipping estimates: https://baymard.com/research-articles/show-shipping-costs-on-product-pages
- Mobile Ecommerce UX: https://baymard.com/research/mcommerce-usability
- Mobile UX Trends 2026: https://baymard.com/research-articles/mobile-ux-ecommerce
- Accounts & Self-Service UX 2026: https://baymard.com/research-articles/accounts-and-self-service-ux-research-2026
- Search query support: https://baymard.com/research-articles/ecommerce-search-query-types
- Shipping/return links in footer: https://baymard.com/research-articles/footer-needs-return-shipping-links

Research themes incorporated include prominent guest checkout, product-finding support, applied filter visibility, product-list scannability, shipping/cost transparency, PDP content discoverability, delivery-date communication, mobile-specific patterns, and post-purchase self-service.

### Nielsen Norman Group (NN/g)

Public report descriptions used to cross-check the scope of ecommerce UX as a multi-domain problem:

- Ecommerce Search, Filters, and Routing Pages: https://www.nngroup.com/reports/ecommerce-ux-search-including-faceted-search/
- Product Pages: https://www.nngroup.com/reports/ecommerce-ux-product-pages-including-reviews/
- Homepages, Navigation, and Product Comparisons: https://www.nngroup.com/reports/ecommerce-ux-homepages-and-category-pages/
- Trust and Credibility: https://www.nngroup.com/reports/ecommerce-ux-trust-and-credibility/
- Ecommerce UX Executive Summary: https://www.nngroup.com/reports/ecommerce-user-behavior-executive-summary/

The package does not reproduce paid NN/g report recommendations. Their public descriptions are used only to validate domain coverage and research framing.

## Accessibility

### W3C / WAI

- WCAG 2.2: https://www.w3.org/TR/wcag/
- What's New in WCAG 2.2: https://www.w3.org/WAI/standards-guidelines/wcag/new-in-22/
- Focus Not Obscured: https://www.w3.org/WAI/WCAG22/Understanding/focus-not-obscured-minimum

Important baselines incorporated:

- WCAG 2.2 AA as the target for critical journeys;
- focus must not be hidden by sticky UI;
- target size minimum exists at WCAG level, while this package uses a stricter 44x44 CSS px project target for primary touch controls where practical;
- authentication should support password managers/copy-paste rather than forcing memory/transcription;
- status messages must be exposed programmatically.

## Performance

### web.dev / Chrome

Use current Core Web Vitals guidance and field measurement practices from https://web.dev/.

Project release targets:

- LCP <= 2.5 s
- INP <= 200 ms
- CLS <= 0.1

The package also follows common web.dev guidance to make the LCP resource discoverable early, avoid lazy-loading the likely LCP image, reserve image/layout dimensions, and optimize responsiveness to interactions.

### Chrome autofill

Checkout form guidance also considers browser autofill behavior and correct semantic input attributes. See Chrome developer documentation: https://developer.chrome.com/docs/identity/autofill

## Security

### OWASP Cheat Sheet Series

- Session Management: https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html
- SQL Injection Prevention: https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html
- Content Security Policy: https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html
- Cross-Site Request Forgery Prevention: https://cheatsheetseries.owasp.org/
- Cross-Site Scripting Prevention: https://cheatsheetseries.owasp.org/
- Password Storage: https://cheatsheetseries.owasp.org/
- File Upload: https://cheatsheetseries.owasp.org/

### PHP Manual

- PDO prepared statements: https://www.php.net/manual/en/pdo.prepare.php
- Session security: https://www.php.net/manual/en/session.security.php
- Password hashing APIs: https://www.php.net/manual/en/book.password.php

## Analytics

### Google Analytics ecommerce events

- Ecommerce measurement: https://developers.google.com/analytics/devguides/collection/ga4/ecommerce

The package uses GA4-style ecommerce event naming as a reference vocabulary where analytics is implemented, while allowing other analytics vendors.

## Search / ecommerce discoverability

### Google Search Central

- Ecommerce best practices: https://developers.google.com/search/docs/specialty/ecommerce
- Ecommerce structured data: https://developers.google.com/search/docs/specialty/ecommerce/include-structured-data-relevant-to-ecommerce
- Product / merchant listing structured data: https://developers.google.com/search/docs/appearance/structured-data/merchant-listing
- Product variants: https://developers.google.com/search/docs/appearance/structured-data/product-variants
- Ecommerce URL structure: https://developers.google.com/search/docs/specialty/ecommerce/designing-a-url-structure-for-ecommerce-sites
- Ecommerce site structure: https://developers.google.com/search/docs/specialty/ecommerce/help-google-understand-your-ecommerce-site-structure

## How to maintain this registry

When updating the skill:

1. prefer primary standards/platform docs for technical requirements;
2. prefer current usability research over trend blogs;
3. record the date of research refresh;
4. never copy proprietary paid research text into the skill;
5. separate a source claim from a project-standard decision;
6. re-check time-sensitive metrics, browser guidance, payment guidance, and SEO documentation before major revisions.
