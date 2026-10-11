---
name: nextauth-neon
description: Auth with next-auth v5 (Auth.js) plus Neon serverless Postgres in Next.js App Router. Use when adding login/sessions, protecting routes, or querying Neon from route handlers/server components.
---

# next-auth v5 + Neon

Packages: `next-auth@^5.0.0-beta`, `@neondatabase/serverless`.

## Auth setup (`auth.ts`)
```ts
import NextAuth from "next-auth";
import Credentials from "next-auth/providers/credentials";

export const { handlers, auth, signIn, signOut } = NextAuth({
  providers: [Credentials({
    async authorize(creds) { /* look up user in Neon, verify hash, return user|null */ },
  })],
  session: { strategy: "jwt" },
  callbacks: {
    jwt({ token, user }) { if (user) token.role = (user as any).role; return token; },
    session({ session, token }) { (session.user as any).role = token.role; return session; },
  },
});
```
`app/api/auth/[...nextauth]/route.ts`: `export const { GET, POST } = handlers;`

Use `await auth()` in server components / route handlers; protect routes with middleware/proxy or an explicit check that redirects.

## Neon
```ts
import { neon } from "@neondatabase/serverless";
const sql = neon(process.env.DATABASE_URL!);
const rows = await sql`SELECT * FROM sessions WHERE tutor_id = ${id}`;
```
- Tagged-template queries are parameterized; never string-concatenate user input.
- Use `Pool`/`Client` (needs `ws` in Node) only for transactions; plain `neon()` is HTTP, one query per call.
- Keep schema/migrations in a `db/` folder; hash passwords (bcrypt/argon2), never store plaintext.
- Required env: `AUTH_SECRET`, `DATABASE_URL`.
