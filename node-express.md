# Clerk with Node.js and Express

Use this guide when the target backend uses Express.

## Install and environment

```bash
npm install @clerk/express
```

```env
CLERK_PUBLISHABLE_KEY=pk_test_...
CLERK_SECRET_KEY=sk_test_...
```

Load environment variables using the existing project convention. Keep the secret server-side.

## Middleware and protected routes

Add the middleware once, near the existing Express middleware setup:

```ts
import express from "express";
import { clerkMiddleware, getAuth } from "@clerk/express";

const app = express();
app.use(express.json());
app.use(clerkMiddleware());

app.get("/api/private", (req, res) => {
  const { isAuthenticated, userId } = getAuth(req);
  if (!isAuthenticated) {
    res.status(401).json({ error: "Unauthorized" });
    return;
  }
  res.json({ userId });
});
```

`clerkMiddleware()` makes auth state available; it does not protect every route automatically. Protect only the private API and page routes that need it.

## Sign-in, sign-up, sign-out, and session state

Express is the backend integration. Add the matching Clerk frontend SDK to the existing frontend: `@clerk/nextjs`, `@clerk/react`, or `@clerk/clerk-js` depending on the detected frontend. Use its `SignIn`, `SignUp`, `UserButton`, and sign-out APIs. Do not build an HTML auth UI in the backend if the project already has a frontend.

Use `getAuth(req)` to determine whether a request is authenticated and read `userId`. Use `clerkClient` from `@clerk/express` only in server code when user details are required.

For TypeScript request typing, the current Clerk guide supports:

```ts
/// <reference types="@clerk/express/env" />
```

Integrate into the existing server without replacing its router, error handling, or frontend. Run its existing build and test commands afterward.
