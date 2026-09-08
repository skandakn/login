# Clerk with Flask

Clerk does not provide a first-party Flask server SDK in the current official quickstart set. Do not copy a Node or Next.js integration into Flask. Use this architecture only when the project needs Clerk on a Flask backend:

1. Use Clerk's current frontend integration for the existing browser UI, usually the JavaScript SDK or a supported React frontend.
2. Send the active Clerk session token to Flask as a Bearer token for cross-origin API calls, or use the Clerk session cookie for a same-origin setup.
3. Verify the token on the Flask server using a maintained JWT/JWKS library and the current Clerk token-verification documentation. Reject missing, expired, wrong-issuer, wrong-audience, or invalid-signature tokens with `401`.
4. Treat the verified `sub` claim as the Clerk user ID. Do not trust a user ID posted in JSON.

## Environment

```env
VITE_CLERK_PUBLISHABLE_KEY=pk_test_...
# Use only the verification configuration required by the chosen backend verifier.
CLERK_JWT_KEY=replace_with_verification_key
```

Never put `CLERK_SECRET_KEY` in browser code. If a server-side Clerk operation is required, isolate it in a supported server service and keep its secret in Flask's server environment.

## Minimal Flask boundary

```py
from flask import request, jsonify

@app.get("/api/private")
def private_route():
    token = request.headers.get("Authorization", "").removeprefix("Bearer ")
    claims = verify_clerk_token_with_current_jwks_configuration(token)
    if claims is None:
        return jsonify(error="Unauthorized"), 401
    return jsonify(user_id=claims["sub"])
```

`verify_clerk_token_with_current_jwks_configuration` is intentionally a placeholder: use the verifier and key-discovery method documented for the current Clerk token format rather than inventing cryptography. Add sign-in, sign-up, sign-out, and session controls in the existing frontend. Preserve Flask routes and templates.
