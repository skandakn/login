# Clerk with React + Vite

Use this guide when the target project is a React SPA built with Vite.

## Install and environment

```bash
npm install @clerk/react
```

```env
VITE_CLERK_PUBLISHABLE_KEY=pk_test_...
```

Only the publishable key belongs in the browser. Do not place a secret key in a Vite variable.

## Provider and auth controls

Merge the provider into the existing `src/main.tsx` or `src/main.jsx`:

```tsx
import { ClerkProvider } from "@clerk/react";

createRoot(document.getElementById("root")!).render(
  <ClerkProvider>
    <App />
  </ClerkProvider>,
);
```

Use the prebuilt controls in an existing header or navigation:

```tsx
import { Show, SignInButton, SignUpButton, UserButton } from "@clerk/react";

<Show when="signed-out">
  <SignInButton />
  <SignUpButton />
</Show>
<Show when="signed-in"><UserButton /></Show>
```

For explicit routes, use the target project's existing router and render `SignIn` or `SignUp` on `/sign-in` and `/sign-up`.

## Auth state and protected UI

```tsx
import { Show, useAuth, useUser } from "@clerk/react";

function PrivatePanel() {
  const { isSignedIn, userId, getToken } = useAuth();
  const { user } = useUser();
  if (!isSignedIn) return <p>Please sign in.</p>;
  return <p>{user?.firstName ?? userId}</p>;
}
```

Use the existing router's route guard for private pages. The Clerk React SDK protects client UI state, not arbitrary backend endpoints. For an API, send a Clerk token and verify it on the server using that server's supported Clerk integration or a properly configured JWT/JWKS verifier.

## Sign-out

```tsx
import { useClerk } from "@clerk/react";

function SignOutButton() {
  const { signOut } = useClerk();
  return <button onClick={() => signOut()}>Sign out</button>;
}
```

Preserve the existing Vite structure and design. Do not add a new router or state-management library just for authentication.
