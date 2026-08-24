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
