# Clerk with WordPress

There is no first-party WordPress Clerk server plugin assumed by this toolkit. Do not install or invent a plugin just because a project uses WordPress.

For a WordPress site, choose one of these explicit architectures:

- Frontend-only: add Clerk's current JavaScript SDK to the existing theme or a narrowly scoped plugin for sign-in, sign-up, sign-out, and visible session state. This does not protect WordPress admin or arbitrary PHP endpoints.
- API boundary: send Clerk session tokens to a separate supported backend or token-verification service, and let that service protect private API operations. Pass only verified user identity into WordPress.
- Custom WordPress integration: verify Clerk tokens in PHP using a maintained JWT/JWKS library and current Clerk token documentation, then gate only the specific REST endpoints or template actions that need protection.

## Environment and safety

```env
CLERK_PUBLISHABLE_KEY=pk_test_...
CLERK_JWT_KEY=replace_with_verification_key
```

Do not put `CLERK_SECRET_KEY` in theme JavaScript, page source, or a committed `wp-config.php`. Store server values through the host's secret configuration.

## Minimal REST boundary

```php
// In a narrowly scoped plugin or existing REST callback.
$token = get_bearer_token_from_request();
$claims = verify_clerk_token_with_current_jwks_configuration($token);
if ($claims === null) {
    return new WP_Error('unauthorized', 'Authentication required', ['status' => 401]);
}
$user_id = $claims['sub'];
```

The verifier is intentionally a placeholder for the current supported JWT/JWKS implementation. Do not use unsigned JWT parsing or trust a posted user ID. Preserve the existing theme, plugins, and WordPress routing.
