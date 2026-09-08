# Universal Clerk Integration Agent Prompt

Copy this prompt into Codex or another coding agent and include this repository URL:

```text
You are integrating Clerk authentication into an existing hackathon project. This repository is a reference toolkit, not an application to copy wholesale.

Before changing anything:

1. Inspect the target project structure, package manifests, lockfiles, entry points, and existing routes.
2. Determine the programming language, framework, frontend architecture, backend architecture, package manager, routing system, environment-variable system, and existing authentication system.
3. Check for existing auth dependencies, middleware, session code, user tables, protected routes, and login UI.
4. If existing authentication is present, briefly assess whether a migration is safe and ask for direction before replacing it.

Then:

5. Open this toolkit's integrations/README.md and select exactly one relevant integration guide.
6. Read only that guide. Follow current official Clerk SDK conventions for the detected platform.
7. Do not change the target project's framework, language, package manager, routing library, or visual design.
8. Integrate the smallest reliable authentication surface for a 3-hour hackathon: sign-in, sign-up, sign-out, session/authenticated-user state, and protection for the project's genuinely private pages/routes/API endpoints.
9. Preserve existing UI and modify existing navigation/layout components instead of adding a duplicate shell.
10. Use the target project's environment-variable convention. Never hardcode Clerk secrets, publishable keys, tokens, passwords, or credentials. Never print existing secret files.
11. Do not add a database, webhook, payment, organization system, custom auth abstraction, or unrelated refactor.
12. For unsupported server frameworks, do not invent an official SDK. Follow the selected guide's frontend-plus-token-verification boundary and use current Clerk token/JWKS documentation.
13. Install only the package(s) required by the selected integration and preserve the existing lockfile/package manager.
14. Run the target project's appropriate typecheck, lint, build, and test commands. Start the app if practical and test sign-in, sign-up, sign-out, and one protected resource.
15. Fix authentication-related errors caused by your changes, but do not rewrite unrelated code.
16. If the target app deploys on Vercel, read `deployment/vercel.md` and configure the target Vercel project only if the user has provided project access and separately supplied the required credentials. A deployment URL alone is not enough.
17. Add Clerk variables to the correct Vercel environments and trigger a new deployment. Never commit `.vercel`, `.env.local`, `VERCEL_TOKEN`, or Clerk secrets.

At the end, report:

- the detected stack and selected toolkit guide;
- exact files changed;
- dependencies added;
- environment variables required, by name only;
- protected routes or API endpoints;
- verification commands and results;
- any remaining setup the user must do in the Clerk Dashboard.
- whether Vercel Preview and Production environments were configured and redeployed.

Prefer the simplest reliable implementation. Keep the change small, reversible, and consistent with the existing project.
```
