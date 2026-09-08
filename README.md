# Universal Clerk Authentication Toolkit

This repository is a lightweight reference toolkit for adding Clerk authentication to an existing hackathon project. It is not a website, dashboard, starter app, or database layer.

Use it with an AI coding agent. The agent should inspect the real project first, choose one integration guide, and make the smallest changes that fit the existing stack.

## How to use this during a hackathon

1. Give the agent the URL of this repository and [the integration prompt](prompts/integration-agent-prompt.md).
2. Give Clerk credentials separately through the target project's environment-variable system.
3. Ask the agent to inspect the target project before changing files.
4. The agent should read only the relevant guide in `integrations/`.
5. The agent should preserve the existing framework, UI, routes, and package manager.
6. For Vercel deployments, follow [deployment/vercel.md](deployment/vercel.md) and add credentials to the target Vercel project, never to this repository.

## Supported frameworks

- Next.js App Router and React with TypeScript: first-party SDK and middleware/proxy support.
- React + Vite: first-party `@clerk/react` SDK.
- Vanilla JavaScript: first-party Clerk JavaScript SDK.
- Node.js + Express: first-party `@clerk/express` SDK.
- Flask, Django, and WordPress: no first-party Clerk server SDK is assumed here. Use the documented frontend-plus-token-verification architecture and verify the current Clerk support before implementing.

See the [integration decision table](integrations/README.md) for the correct guide.

## FAST HACKATHON WORKFLOW

1. Create the actual hackathon project.
2. Determine its language, framework, frontend, backend, router, and package manager.
3. Give the AI agent this repository URL.
4. Give the agent `prompts/integration-agent-prompt.md`.
5. The agent inspects the existing project and existing authentication first.
6. The agent selects the matching guide from `integrations/`.
7. The agent integrates only sign-in, sign-up, sign-out, session state, and route/API protection needed by the project.
8. Add credentials through environment variables. Never paste them into source files.
9. Run the target project's build, type, and test checks.
10. Test sign-in and sign-up locally, then continue building the application.

## Vercel deployment

Vercel environment variables belong to the actual hackathon website's Vercel project, not this toolkit repository. A Vercel URL by itself is not permission to change that project's settings. The user or agent must have access to the Vercel project and provide credentials through the Vercel Dashboard, Vercel CLI login, or a separately managed `VERCEL_TOKEN`.

Follow [deployment/vercel.md](deployment/vercel.md) to add the Clerk variables to Preview and Production, then redeploy. Environment variable changes apply to new deployments, not already-running deployments.

## Security rules

- Never commit `CLERK_SECRET_KEY`, JWT signing keys, private keys, tokens, or passwords.
- Only expose publishable keys in browser code. A publishable key is not a secret, but it still belongs in environment configuration.
- Use `.env.example` only as a placeholder reference.
- If a secret reaches chat, logs, source control, or a client bundle, rotate it before production use.
- Do not add a database, webhook, payment feature, organization model, or custom auth abstraction unless the target project explicitly needs it.

## Providing Clerk credentials

Create the target project's local environment file using the variable names from the selected integration guide. Use the Clerk Dashboard or Clerk CLI to obtain keys. The server secret must remain server-side.

## Repository contents

```text
.
├── .env.example
├── .gitignore
├── README.md
├── integrations/
│   ├── README.md
│   ├── nextjs.md
│   ├── react-vite.md
│   ├── vanilla-js.md
│   ├── node-express.md
│   ├── flask.md
│   ├── django.md
│   └── wordpress.md
├── prompts/
│   └── integration-agent-prompt.md
├── deployment/
│   └── vercel.md
├── examples/
│   └── README.md
└── security/
    └── credentials.md
```

The repository contains documentation and minimal snippets only. It intentionally has no application build, framework lockfile, or real credentials.

## Official Clerk references

- [Quickstarts](https://clerk.com/docs/getting-started/quickstart/overview)
- [Next.js App Router](https://clerk.com/docs/nextjs/getting-started/quickstart)
- [React + Vite](https://clerk.com/docs/react/getting-started/quickstart)
- [JavaScript](https://clerk.com/docs/js-frontend/getting-started/quickstart)
- [Express](https://clerk.com/docs/expressjs/getting-started/quickstart)
- [Clerk CLI](https://clerk.com/docs/cli)
