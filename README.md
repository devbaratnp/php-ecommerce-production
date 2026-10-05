# PHP Ecommerce Production Skill

> Build, audit, harden, and ship production PHP 8 + MySQL/MariaDB ecommerce systems — with research-informed UX, commerce integrity, and release gates.

## Install

**Claude Code**

```bash
git clone https://github.com/devbaratnp/php-ecommerce-production.git ~/.claude/skills/php-ecommerce-production
```

**Codex / OpenCode**

```bash
git clone https://github.com/devbaratnp/php-ecommerce-production.git ~/.agents/skills/php-ecommerce-production
```

**Any agent (project-local)**

```bash
git clone https://github.com/devbaratnp/php-ecommerce-production.git ./skills/php-ecommerce-production
```

Then invoke with: `Use the php-ecommerce-production skill.` See `examples/README.md` for BUILD / IMPLEMENT / AUDIT / FIX / UX / SECURITY / RELEASE prompts.

## What this is

This package expands the original `php-ecommerce` skill into a modular system. It keeps the strongest parts of the original skill (security basics, role-based auth, admin CRUD, cart/checkout patterns, and storefront UX checks) while correcting or deepening areas that need production-grade treatment.

## What this skill is for

Use it when you need to:

- build a new single-vendor ecommerce application;
- turn approved static UI into a functional PHP ecommerce system;
- audit an existing ecommerce codebase;
- repair cart, checkout, stock, auth, order, or admin flows;
- improve storefront UX without redesigning blindly;
- harden security before launch;
- validate mobile, accessibility, performance, analytics, and release readiness;
- adapt one core architecture to different ecommerce verticals such as apparel, grocery, electronics, jewelry, dairy/FMCG, or B2B.

## Default technical profile

The reference implementation assumes:

- PHP 8.x
- MySQL/MariaDB
- PDO with native prepared statements
- PHP sessions for authentication/session state
- server-rendered pages by default
- progressive enhancement with JavaScript
- service-layer business logic
- transactions for critical commerce writes
- mobile-first, accessible storefront and admin

The principles still apply if the UI is Blade, React, Vue, or another renderer. The ecommerce rules should live below the rendering layer.

## Why the package is modular

Ecommerce UX and engineering are too broad for a single long `SKILL.md`. The root skill is therefore an orchestrator. Detailed rules are split into focused references so an agent can load only what it needs.

For example, a product-page task should load the Product Page, Media & Variants, Accessibility, and Performance references rather than carrying the entire checkout and admin rulebook into context.

## Package map

```text
php-ecommerce-production/
├── SKILL.md                         # Agent workflow and non-negotiables
├── README.md                        # Human-facing package guide
├── CHANGELOG.md
├── references/
│   ├── research-sources.md          # Research/source registry
│   ├── architecture/                # Discovery, architecture, data model
│   ├── security/                    # Sessions, auth, CSRF, XSS, SQL, uploads
│   ├── commerce/                    # Catalog, inventory, pricing, cart, orders
│   ├── ux/                          # Full storefront/customer UX system
│   ├── admin/                       # Admin/back-office UX and operations
│   ├── optional/                    # Vertical/optional commerce patterns
│   └── quality/                     # Audit, tests, analytics, SEO, release gates
├── templates/                       # Reusable project/audit documents
├── examples/                        # Invocation and workflow examples
└── legacy/original-SKILL.md         # Original uploaded skill, preserved unchanged
```

## The four labels used throughout the references

- **MUST**: release-critical requirement unless explicitly documented as not applicable.
- **SHOULD**: default recommendation; deviate only for a concrete business or UX reason.
- **MAY**: optional pattern activated by project needs.
- **DO NOT**: known unsafe, deceptive, inaccessible, or fragile pattern.

You will also see two evidence labels:

- **Research-backed**: informed by published UX/accessibility/performance/security research or platform documentation. See `references/research-sources.md`.
- **Project standard**: engineering synthesis chosen for consistency, maintainability, or operational safety. It may be stricter than a published minimum.

## Core philosophy

A storefront is not finished because every screen exists. It is finished when users can complete critical journeys correctly under realistic states, the server protects commerce integrity, and operators can manage the resulting orders and inventory safely.

A visible control must do one of three things:

1. work;
2. clearly communicate that it is unavailable and why; or
3. not be shown.

Never ship fake interactivity, fake reviews, fake stock scarcity, fake countdown timers, fake trust badges, fictional filters, or UI that implies a capability the backend does not support.

## Recommended reading order

For a new project:

1. `SKILL.md`
2. `references/architecture/01-discovery-and-mode-routing.md`
3. `references/architecture/02-system-architecture.md`
4. `references/architecture/03-domain-and-data-model.md`
5. security references
6. commerce references
7. the relevant UX references
8. admin references
9. quality/release references

For an audit, start with `references/quality/01-audit-framework.md` and load modules only where problems are found.

## Important differences from the original skill

The original file is preserved under `legacy/`. This production package intentionally changes several defaults:

- PDO is the default database API instead of MySQLi examples.
- Guest checkout is supported and should normally be prominent; login is not a universal prerequisite for checkout.
- SameSite cookie policy is chosen based on actual payment/OAuth flows rather than hardcoding `Strict` everywhere.
- Checkout/order creation uses database transactions, idempotency, and server-authoritative pricing/stock.
- Payment completion is modeled as a state machine, not a single success boolean.
- Product, cart, checkout, account, and post-purchase UX are separate behavioral contracts.
- Mobile, accessibility, Core Web Vitals, analytics, SEO, and operational observability are release concerns, not polish after launch.
- Ecommerce patterns are activated by store archetype instead of blindly applying the same features to every vertical.
