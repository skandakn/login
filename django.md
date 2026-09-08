# Clerk with Django

Clerk does not provide a first-party Django server SDK in the current official quickstart set. Do not pretend that the Express middleware can be dropped into Django.

Use the existing frontend integration for sign-in, sign-up, sign-out, and session UI. For Django API or page protection, send a Clerk session token to the server and verify it with a maintained JWT/JWKS library using the current Clerk verification guidance. Check signature, expiry, issuer, audience, and the `sub` claim before attaching a user identity to the request.

## Environment

```env
VITE_CLERK_PUBLISHABLE_KEY=pk_test_...
CLERK_JWT_KEY=replace_with_verification_key
```

Use Django's existing environment loader. Never expose a server secret or private verification material to templates or static JavaScript.

## Minimal view boundary

```py
from django.http import JsonResponse

def private_view(request):
    token = request.headers.get("Authorization", "").removeprefix("Bearer ")
    claims = verify_clerk_token_with_current_jwks_configuration(token)
    if claims is None:
        return JsonResponse({"error": "Unauthorized"}, status=401)
    return JsonResponse({"user_id": claims["sub"]})
```

The verifier name is deliberately a placeholder. Select a maintained library and implement it from current Clerk documentation, or place Clerk verification in a small supported service. Do not add a custom authentication protocol or database sync for a short hackathon unless required. Keep existing Django URLs, templates, and middleware intact.
