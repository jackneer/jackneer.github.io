---
title: Publishing
nav_order: 3
---

# Publishing

The site is **live** at **[https://jackneer.github.io](https://jackneer.github.io)**.
It is built and deployed automatically by GitHub Actions on every push to `main`.

## How it works

| Piece | Role |
|---|---|
| `main` branch | The published source. A push here triggers a deploy. |
| `draft` branch | Optional staging area; merge into `main` to publish. |
| `.github/workflows/pages.yml` | Builds Jekyll and deploys to GitHub Pages. |
| Settings → Pages | Source is **GitHub Actions**. |

## Publishing a change

1. Edit or add Markdown under `docs/` (or edit `index.md`, `about.md`, `_config.yml`).
2. Commit and push:
   - **Directly to `main`** — the change goes live in about a minute.
   - **To `draft`**, then merge `draft` → `main` — useful if you want a staging step.
3. Watch progress under the repo's **Actions** tab (*Deploy site to GitHub Pages*).
4. Refresh https://jackneer.github.io.

## Triggering a deploy by hand

Actions tab → **Deploy site to GitHub Pages** → **Run workflow**.

## Rolling back

Revert the offending commit on `main` and push — the site redeploys the previous state.
