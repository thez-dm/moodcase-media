# moodcase-media

**Fallback public image host for ambassador photography used in moodcase Instagram posts.**

> **Fallback only.** AP images now live on the moodcase platform and are found via the moodcase MCP `search-tool`. Each result's `image_url` is a public signed CDN URL (`cdn.previews.moodcase.io`) Buffer fetches directly — those images never touch this repo. This repo hosts the remainder: photos a photographer sent outside the platform. See `moodcase-content-hub/social/instagram/ap-workflow/moodcase-ap/SKILL.md`, Phase 5 Path B.

Buffer can only display an image it can reach at a public URL. Off-platform ambassador photography lives in the private `moodcase-content-hub` repo, so this public repo gives each selected photo a `raw.githubusercontent.com` URL that Buffer's servers can pull. Same role that `moodcase-instagram-cards` plays for quote and hack cards.

## Project type: Git repo (public)

- Repo path: `/Users/Thez/Documents/CLAUDE/CLAUDE COWORK/PROJECTS/moodcase-media`
- GitHub: `thez-dm/moodcase-media` — **must be public** (Buffer fetches the raw URL).

## Structure

```
ap/            ← staged AP photos. A file here = selected and committed to GitHub.
ap/_pushed/    ← AP photos already used in a scheduled Buffer post (their raw URL is live).
```

A photo's location is its state: `ap/` = staged (URL live, waiting for next planner catch-up), `ap/_pushed/` = in Buffer.

Raw URL pattern (used as the Buffer post's image):
`https://raw.githubusercontent.com/thez-dm/moodcase-media/main/ap/<filename>`

## How the monthly planner uses this repo

`instagram-monthly-planner` uses this repo **only for Path B (fallback) photos** — MCP-sourced images skip it entirely. For those fallback photos it:

1. **Catch-up:** for each photo already in `ap/` (staged by the previous run), ensures it is committed to GitHub via API, then schedules it in Buffer as a post, then moves it to `ap/_pushed/` via API.
2. **Stage:** copies this run's newly selected AP photos into `ap/` and immediately commits them to GitHub via the API — raw URLs go live at staging time.

## GitHub API commits — no manual push needed

The planner uses `PROJECTS/moodcase-content-hub/scripts/github_push.py` to commit files to this repo directly via the GitHub REST API. The token lives at `REFERENCES/secrets/github_token.txt` (outside any git repo).

This means:
- You never need to push `moodcase-media` in GitHub Desktop.
- Staged photos are live on GitHub immediately after the planner runs.
- All file moves (ap/ → ap/_pushed/) happen via API as well.

**moodcase-content-hub** (plan files, tracker, ap-library archives) still requires a manual commit + push in GitHub Desktop after each planner run.

## git rule

Do not run `git` from inside the Cowork sandbox. The planner manages this repo entirely via the GitHub REST API. If you ever need to commit manually (edge case), do it in GitHub Desktop.

## Credit

Every photo is ambassador work and is always posted with the photographer's credit line (`| Photography by [Name] · @handle`). The **handle always comes from `ap-workflow/approved-handles.json`** — never from metadata, never inferred. Photographer name and the enriched alt text are read from each image's EXIF/XMP via `write_metadata.py --read-ap` (fallback path only; the platform CDN serves metadata-stripped images).
