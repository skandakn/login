# Clerk with Vanilla JavaScript

Use this guide for a browser app without React or another supported frontend framework.

## Install and environment

For a bundled app:

```bash
npm install @clerk/clerk-js
```

```env
VITE_CLERK_PUBLISHABLE_KEY=pk_test_...
```

For a plain static page, use the current Clerk JavaScript quickstart and its browser bundle pattern instead of inventing a package-loader setup.

## Initialize Clerk

In the existing entry file, initialize one Clerk instance and load it before mounting UI:

```js
import { Clerk } from "@clerk/clerk-js";

const publishableKey = import.meta.env.VITE_CLERK_PUBLISHABLE_KEY;
if (!publishableKey) throw new Error("Missing VITE_CLERK_PUBLISHABLE_KEY");

const clerk = new Clerk(publishableKey);
await clerk.load();

const app = document.querySelector("#app");
if (!app) throw new Error("Missing #app element");

if (clerk.isSignedIn) {
  const userButton = document.createElement("div");
  app.append(userButton);
  clerk.mountUserButton(userButton);
} else {
  const signIn = document.createElement("div");
  app.append(signIn);
  clerk.mountSignIn(signIn);
}
```

For sign-up, mount `clerk.mountSignUp(element)` on the existing sign-up page or view. For sign-out, call `await clerk.signOut()` from the existing account control.

## Auth state and private pages

Use `clerk.isSignedIn`, `clerk.user`, and `clerk.session` after `await clerk.load()`. If the project has a client-side router, guard private views using that state. Client checks alone do not protect a backend.

For API calls, obtain a session token from the Clerk session and send it as a Bearer token. The backend must verify the token using an appropriate official Clerk server SDK or a carefully configured JWT/JWKS verifier.

Keep the existing HTML and CSS. Add only the Clerk initialization and mount points required by the current navigation.
