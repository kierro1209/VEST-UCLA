# VEST Website

Next.js hosted on Vercel.

Use pnpm 10.14.0 as the package manager.

## Vercel deployment

pnpm 10 blocks dependency lifecycle scripts unless they are explicitly approved.
If Vercel fails during `pnpm install` with an `approve-builds` message, run
`pnpm approve-builds` locally, approve the trusted build dependencies, and commit
the generated `pnpm-workspace.yaml` file before redeploying.

README-only changes can trigger a new deployment, but they cannot resolve this
install-time approval requirement by themselves.
