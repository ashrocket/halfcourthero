# Half Court Hero Codex Notes

## Source of Truth

- This repo owns the web game deployed at `https://lizzymcguire.narasroom.bandmusicgames.party`.
- `../bandmusicgames` owns the native iOS port. Use this repo as reference source and shared art source when porting into Swift/SwiftUI, but do not place native iOS code here.

## Deploy

- Local dev: `npm run dev`
- Cloudflare Pages deploy: `npm run deploy`
- GitHub Actions deploys `main` with project name `halfcourthero-narasroom`.

## Repo Hygiene

- Keep generated files out of git: `.wrangler/`, `node_modules/`, `.DS_Store`, and `.claude/*.lock` stay ignored.
- Commit web gameplay and web art changes here before handing off or deploying.
