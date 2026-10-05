# Authentication, Sessions, and RBAC

## Customer authentication goals

A good ecommerce auth system should:

- allow returning customers to sign in reliably;
- not block guest purchase unnecessarily;
- return the customer to their intended destination when safe;
- preserve/merge guest cart state;
- support password reset without account takeover risk;
- expose clear signed-in/signed-out/expired states;
- avoid account enumeration where practical.

## Registration

Minimum data is normally preferable.

Do not ask for shipping address, date of birth, or other fields during account creation unless the business truly needs them at that moment.

Password handling:

```php
$hash = password_hash($password, PASSWORD_DEFAULT);

if (password_verify($password, $user['password_hash'])) {
    // authenticated
}
```

Use `password_needs_rehash()` during successful login when algorithm/cost changes.

## Login

Flow:

1. validate CSRF if browser form;
2. normalize email/identifier according to project rules;
3. fetch account;
4. verify password;
5. verify account status;
6. regenerate session ID;
7. set minimal session identity/role data;
8. merge/associate guest cart safely;
9. redirect to validated intended destination or account/home.

Avoid distinct error messages that reveal whether an email is registered when enumeration is a concern. A generic "Email or password is incorrect" is safer.

## Intended destination

When a signed-out customer tries to access a protected account route:

- store an internal relative path;
- validate it before redirect;
- clear it after use.

Do not use it to force checkout to require authentication.

## Guest checkout

Guest checkout is a first-class path.

The order can store guest contact/address snapshots with `customer_id = NULL` or an equivalent guest owner model.

After purchase, MAY offer:

- "Create an account to track this and future orders";
- secure linking of the just-created order after ownership verification.

Never create an account silently without clear user intent.

## Cart merge

When a guest signs in:

- merge by purchasable/variant ID;
- define whether quantities add or max out;
- enforce stock/max-quantity rules;
- reprice;
- deduplicate;
- show a small confirmation if the resulting cart materially changed.

Cart merge must be idempotent so retries do not double quantities.

## Logout

- clear authenticated session identity;
- rotate/destroy session appropriately;
- invalidate remember-me token if used;
- preserve a guest cart only if explicitly designed and safe.

## Password reset

Use a random, single-use, expiring token.

Store a hash of the token server-side rather than the raw reset token where practical.

Flow:

1. generic reset request response;
2. create token with expiry;
3. email link over HTTPS;
4. verify token + account + expiry;
5. accept new password;
6. invalidate token and relevant old sessions/remember tokens according to policy;
7. notify customer of password change.

Do not include the password in email.

## Admin/staff authentication

Admin authentication should be separate in navigation and guarded on every privileged route.

For higher-risk deployments consider:

- MFA/OTP/passkey support;
- stricter session expiry;
- IP/device anomaly monitoring;
- audit log for privileged actions.

Do not rely on `/admin` being obscure.

## RBAC/capabilities

For a small store, `admin` vs `customer` may be enough. For larger operations use capabilities.

Example:

```text
Owner:
- all permissions

Catalog manager:
- view/edit products
- manage inventory
- no refunds
- no staff administration

Order operator:
- view orders
- update fulfillment
- initiate allowed cancellations

Support:
- view customer/order details
- create support notes
- no pricing/config changes
```

Check permissions server-side through one policy/helper layer.

## Sensitive customer resources

Order route should enforce ownership:

```text
/account/orders/{publicOrderId}
```

Lookup condition should include authenticated customer ownership, not simply `WHERE order_number = ?`.

Guest tracking must use an unguessable token or multi-factor order lookup appropriate to the privacy risk. Order number alone is often too guessable.

## Session expiry UX

If a session expires:

- preserve non-sensitive context where possible;
- explain that sign-in expired;
- after reauthentication return to the safe intended step;
- avoid discarding an entire filled checkout unnecessarily.

Do not preserve sensitive payment fields yourself.

## Auth acceptance tests

- [ ] Successful login regenerates session ID.
- [ ] Invalid credentials do not reveal unnecessary account existence info.
- [ ] Protected customer records enforce ownership.
- [ ] Admin route guard exists on every route/action.
- [ ] Guest checkout works without account creation where applicable.
- [ ] Guest cart survives/merges safely on login.
- [ ] Password reset tokens expire and cannot be reused.
- [ ] Open redirect is prevented.
- [ ] Logout invalidates authenticated state.
- [ ] Password managers/paste/autofill are not blocked.
