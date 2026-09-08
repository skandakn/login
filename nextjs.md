# Clerk with Next.js App Router

Use this guide when the target project has `next` and the App Router.

## Install

```bash
npm install @clerk/nextjs
```

Use the equivalent command for the existing package manager. Current Clerk guidance uses `proxy.ts` for Next.js 16+ and `middleware.ts` for Next.js 15 and below.

## Environment

```env
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_...
CLERK_SECRET_KEY=sk_test_...
```

Keep the secret key on the server. Do not add it to client components or browser-exposed variables.

## Provider and controls

Merge this into the existing root layout. Preserve the existing HTML, metadata, and styling.

```tsx
import { ClerkProvider, Show, UserButton } from "@clerk/nextjs";

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>
        <ClerkProvider>
          <header>
            <Show when="signed-in"><UserButton /></Show>
          </header>
          {children}
        </ClerkProvider>
      </body>
    </html>
  );
}
```

Create public auth routes:

```tsx
// app/sign-in/[[...sign-in]]/page.tsx
import { SignIn } from "@clerk/nextjs";
export default function SignInPage() { return <SignIn />; }
```

```tsx
// app/sign-up/[[...sign-up]]/page.tsx
import { SignUp } from "@clerk/nextjs";
export default function SignUpPage() { return <SignUp />; }
```

## Proxy and protected routes

Add a root `proxy.ts` for Next.js 16+ or name it `middleware.ts` on older Next.js versions:

```ts
import { clerkMiddleware, createRouteMatcher } from "@clerk/nextjs/server";

const isProtected = createRouteMatcher(["/dashboard(.*)", "/api/private(.*)"]);

export default clerkMiddleware(async (auth, request) => {
  if (isProtected(request)) await auth.protect();
});

export const config = {
  matcher: [
    "/((?!_next|[^?]*\\.(?:html?|css|js(?!on)|jpe?g|webp|png|gif|svg|ttf|woff2?|ico|csv|docx?|xlsx?|zip|webmanifest)).*)",
    "/(api|trpc)(.*)",
    "/__clerk/(.*)",
  ],
};
```

The middleware only enables auth state by default. Add the real private paths explicitly.

## Read the current user

In a Server Component or route handler:

```tsx
import { auth, currentUser } from "@clerk/nextjs/server";

const { isAuthenticated, userId } = await auth();
const user = await currentUser();
```

In a Client Component, use `useAuth()` from `@clerk/nextjs`. Always `await auth()` on the server.

## Sign-out and session state

`UserButton` includes the standard sign-out flow. For custom client UI:

```tsx
"use client";
import { useClerk } from "@clerk/nextjs";

export function SignOutButton() {
  const { signOut } = useClerk();
  return <button onClick={() => signOut()}>Sign out</button>;
}
```

Do not rewrite the existing app. Add the provider, auth routes, user control, and route checks at the smallest appropriate ownership boundaries.
