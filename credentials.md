# Credentials and Secret Handling

## Required rule

Never commit real Clerk credentials. This includes publishable keys in projects where configuration is expected to remain private, secret keys, JWT signing keys, PEM keys, session tokens, OAuth credentials, passwords, and copied `.env` files.

## Variable placement

- Browser variables may contain only the Clerk publishable key and must use the target bundler's public prefix.
- Server-only variables include `CLERK_SECRET_KEY` and token-verification material.
- Do not pass server secrets through HTML, client-side configuration, JSON responses, or public build variables.

## Local development

Copy the relevant names from `.env.example` into the target project's ignored local environment file. Use the existing secret manager in CI and deployment. Confirm the secret file is ignored before running `git add`.

## If a secret leaks

Stop using it, rotate or revoke it in Clerk, remove it from commits and logs where possible, and replace the deployment value. Do not paste the replacement into source control or chat.

## Token verification

Never decode a JWT and treat the decoded payload as trusted. Verify its signature and relevant claims using a maintained library and the current Clerk documentation. Reject missing, expired, malformed, wrong-issuer, wrong-audience, and invalid-signature tokens.
