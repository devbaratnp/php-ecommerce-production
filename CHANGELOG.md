# Changelog

## 2.0.0 — 2026-09-21

Major redesign of the original `php-ecommerce` skill into a production package.

### Added

- task-mode router for BUILD, IMPLEMENT, AUDIT, FIX, UX, SECURITY, and RELEASE tasks;
- ecommerce archetype detection and vertical activation rules;
- PDO-first architecture and service-layer guidance;
- transactional order/inventory rules and idempotency guidance;
- detailed catalog, inventory, pricing, promotion, payment, fulfillment, return, and refund references;
- modular UX references for navigation, search, PLP, PDP, cart, checkout, self-service, post-purchase, mobile, accessibility, performance, content, states, and trust;
- admin UX and back-office operations references;
- analytics, observability, SEO/discoverability, audit, journey test, and release-gate references;
- reusable templates and example prompts;
- research/source registry;
- original uploaded skill preserved under `legacy/`.

### Changed

- default DB examples moved from MySQLi to PDO;
- checkout no longer universally requires login;
- cookie/SameSite rules made compatible with real payment and OAuth flows;
- product/list/search guidance now varies by ecommerce archetype;
- "project complete" requires journey and quality gates, not page existence.
