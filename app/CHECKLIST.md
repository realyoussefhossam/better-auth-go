- [x] remove files from `public/*`
- [x] clear `globals.css`
- [x] clear `page.tsx`
- [x] install shadcn `npx shadcn@latest init`
- [x] install components `npx shadcn@latest add button label input sonner`
- [x] show button and test `dev` server

== PART 1 ==

- [x] install Better Auth `npm install better-auth`
- [x] create `.env` and set Environment Variables
- [x] create `lib/auth.ts`
- [x] setup `postgres` database with `neon.tech`
- [x] install prisma `npm install prisma --save-dev`
- [x] initialize prisma `npx prisma init`
- [x] create **Jwks** Model
- [x] push database changes `npx prisma db push`
- [x] add `generated` to `.gitignore`
- [x] adjust **scripts** in `package.json`

- [x] create single Prisma Client in `lib/prisma.ts` (v7: imports from `@/lib/generated/prisma/client`, uses `@prisma/adapter-pg` driver adapter)
- [x] setup prisma adapter with better-auth (lib/auth.ts uses `prismaAdapter` from `better-auth/adapters/prisma` with the singleton `prisma` client, provider `postgresql`)
- [x] generate auth tables `npx @better-auth/cli generate --output=auth.schema.prisma` (installed `@prisma/client@7` first — generated client imports `@prisma/client/runtime/client`)
- [x] make tweaks to `schema.prisma` (merged User/Session/Account/Verification into schema.prisma, kept v7 generator `output` + datasource without `url`)
- [x] quick walkthrough the models:
  - `User` — id, name, email (unique), emailVerified, image, timestamps; has sessions + accounts
  - `Session` — id, expiresAt, token (unique), ipAddress, userAgent, userId→User (cascade); index on userId
  - `Account` — id, accountId, providerId, userId→User (cascade), OAuth tokens, password; index on userId
  - `Verification` — id, identifier, value, expiresAt; index on identifier
- [x] push database changes `npx prisma db push` (Neon `neondb` in sync)
- [x] create Mount Handler in `app/api/auth/[...all]/route.ts` (uses `toNextJsHandler(auth)` from `better-auth/next-js` — verified compatible with Next.js 16 route handler convention)
- [x] adjust `eslint.config.mjs` to ignore generated client (corrected path to `lib/generated/**` — checklist's `/src/generated/**/*` didn't match this project's layout)
- [x] create Client instance in `lib/auth-client.ts` (`createAuthClient` from `better-auth/react`, `baseURL: process.env.NEXT_PUBLIC_API_URL`)