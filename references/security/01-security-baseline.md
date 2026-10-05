# Security Baseline

## Scope

This is a practical baseline for ordinary PHP ecommerce applications. It is not a substitute for a dedicated penetration test where risk warrants one.

## Threat surfaces to consider

- authentication and sessions;
- account enumeration/brute force;
- authorization/IDOR;
- CSRF;
- XSS;
- SQL injection;
- file uploads;
- open redirects;
- password reset;
- admin actions;
- payment callbacks/webhooks;
- secrets/config;
- logs;
- dependency/server configuration;
- email links/tokens;
- checkout/order manipulation.

## Session cookie baseline

Set session configuration before `session_start()`.

Typical production baseline:

```php
ini_set('session.use_strict_mode', '1');
ini_set('session.use_only_cookies', '1');

session_set_cookie_params([
    'httponly' => true,
    'secure'   => true,       // HTTPS production
    'samesite' => 'Lax',      // choose based on real flow
    'path'     => '/',
]);

session_start();
```

### SameSite decision

Do not blindly force `Strict`.

- `Strict`: strongest cross-site restriction but can break expected flows returning from external sites.
- `Lax`: common default for ordinary first-party ecommerce navigation while still reducing many cross-site request risks.
- `None`: only when cross-site cookie behavior is truly required; requires `Secure` and should be carefully justified.

Payment provider and OAuth return flows must be tested with the chosen policy.

SameSite is defense-in-depth, not a replacement for CSRF protection.

## Session fixation

After successful login, registration that creates an authenticated session, or meaningful privilege elevation:

```php
session_regenerate_id(true);
```

Also regenerate after sensitive account state changes where appropriate.

## Session lifetime

Define:

- inactivity timeout;
- absolute lifetime for privileged/admin sessions if needed;
- logout behavior;
- "remember me" as a separate securely designed token, not an infinite PHP session cookie.

Admin sessions may require stricter limits than storefront carts.

## Authorization

Authentication answers "who are you?" Authorization answers "may you do this?"

Every privileged request must check authorization on the server.

Never rely on:

- hidden input fields;
- disabled buttons;
- route obscurity;
- JavaScript;
- a role label sent by the browser.

Example:

```php
function requireAdmin(): void {
    if (empty($_SESSION['user_id']) || ($_SESSION['role'] ?? null) !== 'admin') {
        http_response_code(403);
        exit('Forbidden');
    }
}
```

For multiple staff roles, use capability/policy checks rather than increasingly complex `if role === ...` scattered through pages.

## CSRF

State-changing browser requests authenticated by cookies must use CSRF protection.

Generate a cryptographically random token associated with the session and compare with `hash_equals()`.

Do not mutate via GET.

Consider per-session or per-form tokens according to project complexity. Rotate tokens carefully so multi-tab checkout forms remain usable.

## SQL injection

Use PDO prepared statements for values:

```php
$stmt = $pdo->prepare('SELECT id, email FROM users WHERE email = :email');
$stmt->execute(['email' => $email]);
```

Prepared placeholders cannot safely replace SQL identifiers such as column names or `ORDER BY` direction. For dynamic identifiers, map user choices to an explicit allowlist.

```php
$sortMap = [
    'price_asc' => 'price ASC',
    'price_desc' => 'price DESC',
    'newest' => 'created_at DESC',
];
$orderBy = $sortMap[$requestedSort] ?? $sortMap['newest'];
```

## XSS/output encoding

Default HTML text output:

```php
<?= htmlspecialchars($value, ENT_QUOTES | ENT_SUBSTITUTE, 'UTF-8') ?>
```

Encoding is context-specific. HTML text, attributes, JavaScript, CSS, and URL contexts are not interchangeable.

Avoid injecting untrusted data into inline JavaScript. Prefer data attributes or JSON encoded safely.

Rich HTML descriptions require an allowlist sanitizer, not simply "trusted because admin entered it."

## Security headers

Use appropriate headers in production, considering required third-party services.

Common baseline:

```text
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
Content-Security-Policy: ...
Strict-Transport-Security: ...  (HTTPS only, deploy carefully)
Permissions-Policy: ...
```

CSP should preferably avoid broad `unsafe-inline` rules. Use nonces/hashes where feasible.

Frame protection can be set with CSP `frame-ancestors`; use `X-Frame-Options` as compatible defense-in-depth where appropriate.

## Error handling

Production:

- do not display stack traces/SQL errors;
- log exceptions securely;
- return generic user-facing error pages/messages;
- attach a correlation/request ID to logs and safe error messages if useful.

Never log:

- passwords;
- password reset raw tokens;
- full session IDs;
- payment card data;
- secrets;
- unnecessary identity documents.

## Rate limiting / abuse controls

Rate-limit or otherwise protect:

- login;
- registration;
- password reset;
- OTP send/verify;
- coupon brute force if sensitive;
- review submission;
- contact/support forms;
- expensive search/API endpoints;
- guest order lookup.

Design limits by IP + account/device/context, avoiding easy denial of service against legitimate users.

## Open redirects

Never redirect to arbitrary `next`/`return` URL supplied by a user.

Allow only relative internal routes or validate against an allowlist.

## Sensitive operations

Consider reauthentication for:

- password/email changes;
- adding/changing payout/payment data;
- staff privilege elevation;
- viewing highly sensitive exports;
- deleting account data.

## Secrets

Store secrets in environment/server secret mechanisms outside public web root.

Rotate exposed credentials immediately; deleting them from the latest Git commit is not enough if they remain in history.

## Security acceptance checklist

- [ ] HTTPS production configuration known.
- [ ] Secure/HttpOnly/SameSite session cookie policy tested.
- [ ] Session ID regenerated after authentication.
- [ ] CSRF on all relevant mutations.
- [ ] No state-changing GET endpoints.
- [ ] Prepared statements for untrusted DB values.
- [ ] Dynamic SQL identifiers are allowlisted.
- [ ] Output encoding/sanitization strategy exists.
- [ ] Authorization enforced server-side.
- [ ] Uploads validated/stored safely.
- [ ] Password reset tokens are secure and expiring.
- [ ] Sensitive endpoints have abuse protection.
- [ ] Production errors are not disclosed to users.
- [ ] Secrets are outside repo/public root.
- [ ] Payment/webhook authenticity is verified.
