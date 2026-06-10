# moodcase-media

**Public image host for ambassador photography used in moodcase Instagram Buffer ideas.**

Buffer can only display an image it can reach at a public URL. Ambassador photography lives in the private `moodcase-content-hub` repo, so this public repo gives each selected photo a `raw.githubusercontent.com` URL that Buffer's servers can pull when an idea is created. Same role that `moodcase-instagram-cards` plays for quote and hack cards.

## Project type: Git repo (public)

- Repo path: `/Users/Thez/Documents/CLAUDE/CLAUDE COWORK/PROJECTS/moodcase-media`
- GitHub: `thez-dm/moodcase-media` — **must be public** (Buffer fetches the raw URL).
- Owner action (one-time): create the public repo on GitHub and connect it in GitHub Desktop.

## Structure

```
ap/            ← staged AP photos. A file here = selected, not yet pushed to Buffer.
ap/_pushed/    ← AP photos already pushed as a Buffer idea (their raw URL is live).
```

A photo's location is its state: `ap/` = staged, `ap/_pushed/` = in Buffer.

Raw URL pattern (used as the Buffer idea's image):
`https://raw.githubusercontent.com/thez-dm/moodcase-media/main/ap/<filename>`

## How the monthly planner uses this repo

`instagram-monthly-planner` (in `moodcase-content-hub`) runs on the 25th and:

1. **Catch-up:** pushes the PREVIOUS run's staged photos (`ap/`) — now live on GitHub — to Buffer as ideas, then moves each into `ap/_pushed/`.
2. **Stage:** copies this run's newly selected AP photos into `ap/` for next time.

Because the Cowork sandbox cannot push to GitHub, a photo staged on run N reaches Buffer on run N+1, after you have pushed this repo. To flush sooner, push this repo in GitHub Desktop and re-run the planner ("Run now").

## Git rule

Do not run `git` from inside the Cowork sandbox (the mount blocks file deletion, which corrupts `.git` lock files). The planner only **creates, copies, and moves** files here. You commit and push in GitHub Desktop — that is what makes each photo's raw URL live.

## Credit

Every photo is ambassador work and is always posted with the photographer's handle (carried in the Buffer idea copy). Handles + descriptions are the source of truth in `moodcase-content-hub/social/instagram/ap-library/ambassador-photography-selection-2026.xlsx`.
