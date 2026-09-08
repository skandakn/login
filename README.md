# Integration Decision Table

Inspect the target project before choosing a guide.

| Target project | Read | Official package or approach | Credentials |
| --- | --- | --- | --- |
| Next.js App Router | `nextjs.md` | `@clerk/nextjs` | Publishable key plus server secret |
| React + Vite | `react-vite.md` | `@clerk/react` | Vite publishable key |
| Vanilla browser JavaScript | `vanilla-js.md` | `@clerk/clerk-js` | Publishable key |
| Node.js + Express | `node-express.md` | `@clerk/express` | Publishable key plus server secret |
| Flask | `flask.md` | No first-party Flask server package assumed | Publishable key plus backend verification configuration |
| Django | `django.md` | No first-party Django server package assumed | Publishable key plus backend verification configuration |
| WordPress | `wordpress.md` | No first-party WordPress package assumed | Publishable key plus backend verification configuration |

Do not install every package. Read one guide, follow the target project's package manager, and adapt the examples to its existing routing and UI.
