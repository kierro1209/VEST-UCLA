# VEST Website

Next.js hosted on Vercel.

Use pnpm 10.14.0 as the package manager.

## Vercel deployment

pnpm 10 blocks dependency lifecycle scripts unless they are explicitly approved.
If Vercel fails during `pnpm install` with an `approve-builds` message, run
`pnpm approve-builds` locally and approve `sharp` and `unrs-resolver`. The approved
dependencies are recorded in `pnpm-workspace.yaml` before redeploying.

The repository already records these trusted build dependencies for Vercel.

The leaderboard uses `NEXT_PUBLIC_SUPABASE_URL` and
`NEXT_PUBLIC_SUPABASE_ANON_KEY`. Add both variables to the Vercel project's
Environment Variables for leaderboard data to appear; without them, the page
builds and displays an empty-state message.

## Member Portal

Routes (under `src/app/(home)/members/`):

- `/members` — searchable directory of the current class
- `/members/[slug]` — individual member profile
- `/members/alumni` — alumni directory
- `/members/login` — gated sign-in (contact info reveal)
- `/members/accolades` — links back to main VEST site sections

Key files:

- `src/data/members.ts` — static seed data (current source of truth in v1)
- `src/lib/members.ts` — async data layer (`getAllMembers`, `getMember`, `searchMembers`, etc.)
- `src/lib/auth.tsx` — `AuthProvider` + `useAuth()` stub (localStorage-backed)
- `src/components/Members/*` — `MemberCard`, `MemberDirectory`, `MemberProfile`, `PortalShell`

### v1 → Supabase migration plan

The data layer and auth hook are intentionally async + dependency-free so we
can swap them to Supabase without touching UI components.

1. Create a `members` table mirroring the `Member` shape in `src/data/members.ts`
   (plus `experiences` and `interests` join tables, or `jsonb` columns to start).
2. Replace the bodies in `src/lib/members.ts` with `supabase.from('members')…`
   queries — keep the function signatures.
3. In `src/lib/auth.tsx`, replace the stub `signIn` with
   `supabase.auth.signInWithOtp` (magic link) or Google OAuth, and source `user`
   from `supabase.auth.getUser()` / `onAuthStateChange`.
4. Add Supabase RLS so contact info columns are only selectable by authenticated
   members, and drop the `MEMBER_DOMAINS` allowlist.

`@supabase/supabase-js` is already installed and a client lives at
`src/lib/supabase.ts` (needs `NEXT_PUBLIC_SUPABASE_URL` and
`NEXT_PUBLIC_SUPABASE_ANON_KEY` in `.env.local`).

### Branch / PR rules

Per the team workflow: **no direct commits to `master`**. Work on a feature
branch (e.g. `feat/member-portal`) and open a PR.
