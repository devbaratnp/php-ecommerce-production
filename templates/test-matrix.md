# Ecommerce Test Matrix

| ID | Area | Preconditions | Steps | Expected | Result | Evidence/Issue |
|---|---|---|---|---|---|---|
| J1 | Guest purchase | Product in stock | Browse -> cart -> guest checkout | One correct order | | |
| J2 | Search | Search enabled | Search -> filter -> PDP | Context preserved | | |
| J3 | No results | None | Impossible query | Recovery routes shown | | |
| J4 | Variant | Variant unavailable | Select invalid combo | Unavailable state clear | | |
| J5 | Stock | 1 unit remains | 2 sessions checkout | Only valid stock sold | | |
| J6 | Reprice | Cart has item | Change price -> checkout | New price surfaced | | |
| J7 | Idempotency | Valid cart | Double submit | One order | | |
| J8 | Payment fail | Gateway sandbox | Force failure -> retry | Recover without duplicate | | |
| J9 | Webhook | Valid event | Deliver twice | One state transition | | |
| J10 | Returning user | Existing guest cart | Login -> merge | Deterministic merged cart | | |
| J11 | Guest tracking | Guest order | Use token | Own order only | | |
| J12 | Cancel | Eligible/ineligible orders | Cancel | Correct consequences | | |
| J13 | Return/refund | Delivered order | Return -> refund | States/stock/payment correct | | |
| J14 | Admin fulfill | New order | Pack -> ship | Valid transitions/audit | | |
| A1 | Keyboard | Storefront available | Complete purchase by keyboard | Pass | | |
| A2 | Zoom | 200%/reflow | Purchase path | No hidden controls | | |
| M1 | Mobile filter | Mobile viewport | Apply/clear filters | State stable | | |
| P1 | Performance | Mobile lab | Measure PDP | LCP/INP/CLS recorded | | |

## Result values

- PASS
- FAIL
- BLOCKED
- UNVERIFIED
- NOT APPLICABLE
