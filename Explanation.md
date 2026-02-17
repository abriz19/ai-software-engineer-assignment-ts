# Explanation

## What was the bug?

The OAuth refresh logic in `HttpClient.request()` used an incorrect conditional:

```
if (
  !this.oauth2Token ||
  this.oauth2Token instanceof OAuth2Token ||
  this.oauth2Token.expired
)
```

This logic caused two incorrect behaviors:

1. A valid OAuth2 token was always refreshed unnecessarily.
2. A plain object token was treated as valid, so no refresh occurred and the Authorization header was never set.

The test case using a plain object token exposed this failure.

---

## Why did it happen?

The condition incorrectly refreshed when the token _was already a valid OAuth2Token_. Additionally, the code attempted to read `.expired` without guaranteeing the token was actually an OAuth2Token, leading to unsafe logic and skipped refresh behavior.

The conditional mixed type checking and expiration logic in a way that did not correctly represent the intended rules.

---

## Why does the fix solve it?

The condition was replaced with:

```
if (
  !(this.oauth2Token instanceof OAuth2Token) ||
  this.oauth2Token.expired
)
```

This ensures refresh occurs only when:

- the token is missing or invalid, or
- the token is expired.

The `instanceof` check safely narrows the type before accessing `.expired`, preventing incorrect behavior and guaranteeing a valid Authorization header is produced.

All tests now pass:

- valid tokens are reused,
- missing tokens are refreshed,
- invalid tokens are refreshed safely.

---

## Remaining edge case not covered

The tests do not simulate a token expiring during a request (e.g., clock skew or near-expiry timing). In real systems, a safety buffer is often used to refresh slightly early. This timing-related edge case is outside the scope of the current tests.
