# Vercel Environment Setup

Use this guide for the actual hackathon website's Vercel project. Do not add Clerk keys to this toolkit repository, GitHub source files, `vercel.json`, client code, or committed environment files.

## Required access

A deployment URL is only a URL. It does not grant permission to edit environment variables. To configure Vercel, the user or agent needs one of:

- access to the Vercel project in the Dashboard;
- an authenticated Vercel CLI session; or
- a separately supplied `VERCEL_TOKEN` with access to the target project/team.

Never commit `VERCEL_TOKEN` or put it in a browser-exposed variable.

## Variable names

Use the names required by the selected integration guide. For Next.js:

```text
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY
CLERK_SECRET_KEY
```

For React/Vite or vanilla JavaScript, the browser variable is usually:

```text
VITE_CLERK_PUBLISHABLE_KEY
```

Express and other server integrations may use `CLERK_PUBLISHABLE_KEY` and `CLERK_SECRET_KEY`. Do not create every variable automatically; add only the names used by the target app.

## Dashboard workflow

1. Open the actual website's Vercel project.
2. Open **Settings > Environment Variables**.
3. Add the publishable key to Preview and Production as needed.
4. Add the secret key to Preview and Production, mark it sensitive when available, and keep it server-only.
5. Save the variables.
6. Redeploy the target project. Vercel applies changed environment variables to new deployments, not previous deployments.

Use separate Clerk development and production instances when the project needs different credentials per environment.

## CLI workflow

Run these commands from the actual website project, after installing and authenticating the Vercel CLI:

```bash
vercel link
vercel env add NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY preview
vercel env add NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY production
vercel env add CLERK_SECRET_KEY preview --sensitive
vercel env add CLERK_SECRET_KEY production --sensitive
vercel env ls preview
vercel env ls production
vercel deploy --prod
```

Each `vercel env add` command prompts for its value. Enter the value directly into the prompt; do not put it in shell history, source code, a script, or a chat message. Replace the variable names with the names from the selected integration guide.

For a non-production preview deployment, deploy normally after adding the Preview variables:

```bash
vercel deploy
```

After changing a Vercel variable, always trigger a fresh deployment. Verify the running deployment through the app's sign-in, sign-up, sign-out, and one protected route/API request.

## Agent safety checklist

- Confirm the target Vercel project before changing variables.
- Do not infer the project from a URL if the user has not granted access.
- Do not read or print existing `.env` files.
- Do not put `CLERK_SECRET_KEY` in a `NEXT_PUBLIC_*` or `VITE_*` variable.
- Do not return secret values in the final report; report names and environments only.
- If Vercel access is missing, stop after documenting the exact variables the user must add.
