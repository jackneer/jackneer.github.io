# AGENTS.md

Guidance for AI coding agents (and humans) working in this repository.

## Project overview

- **What it is:** "Jack Wu's Docs" — a personal knowledge base / documentation site.
- **Live URL:** https://jackneer.github.io
- **Repository:** `jackneer/jackneer.github.io` (public).
- **Hosting:** GitHub Pages, deployed by GitHub Actions.
- **Authoring model:** every page is a plain Markdown file with YAML front matter.

## Tech stack

| Layer | Choice |
|---|---|
| Static site generator | Jekyll (GitHub Pages runs `github-pages` v232 / Jekyll 3.10.0 in CI) |
| Theme | [`just-the-docs/just-the-docs`](https://github.com/just-the-docs/just-the-docs), pinned via `remote_theme` in `_config.yml` |
| Content format | Markdown (`.md`) |
| Deployment | GitHub Actions — `.github/workflows/pages.yml` |
| Pages build type | `workflow` (Actions), **not** legacy branch deploy |

> **Theme pinning:** `remote_theme: just-the-docs/just-the-docs@<tag>` must point to a
> **tag that actually exists**. Before bumping, confirm it at
> <https://github.com/just-the-docs/just-the-docs/tags> and that the release still
> supports Jekyll `>= 3.8.5` (GitHub Pages uses Jekyll 3.10.0). A missing tag makes
> the remote-theme download return `404` and the build fails.

## Repository layout

| Path | Purpose |
|---|---|
| `_config.yml` | Site configuration: title, `remote_theme`, search, footer, `exclude` |
| `index.md` | Home page (`permalink: /`) |
| `docs/` | Knowledge base content |
| `docs/guides.md` | Example section page (`has_children: true`) |
| `docs/guides/example-page.md` | Example child page |
| `about.md` | About page |
| `AGENTS.md` | This file (excluded from the built site) |
| `README.md` | Human-facing overview and authoring guide |
| `LICENSE` | MIT |
| `.github/workflows/pages.yml` | Build + deploy pipeline |
| `_site/` | Jekyll build output — **gitignored, never edit or commit** |

## Branching and deploy model

| Branch | Role |
|---|---|
| `main` | **Published.** Every push triggers a build + deploy. |
| `draft` | Optional staging branch. Merge into `main` to publish. |

- Fastest path: commit directly to `main`.
- Staging path: commit to `draft`, then merge `draft` → `main`.
- When you change content, **keep `main` and `draft` in sync** so the staging branch
  never silently regresses the live site.
- A push to `draft` alone does **not** deploy (the workflow only runs on `main`).

## Authoring content

Every page needs a front-matter block:

```yaml
---
title: Title shown in the sidebar   # required
nav_order: 5                        # sidebar position, lower = higher
# has_children: true                # add on section pages that hold sub-pages
# parent: Section Title             # add on child pages (must match a section's title)
# permalink: /custom-url/           # optional
---
```

### Sections and nesting

- A **section** page sets `has_children: true`.
- A **child** page sets `parent:` to the section's `title`.
- Children are ordered within the section by their own `nav_order`.
- Nesting deeper: a child can itself set `has_children: true`.

### Conventions

- Put content under `docs/`; use **kebab-case** filenames (`writing-pages.md`).
- Prefer `{% link docs/page.md %}` for internal links so they survive URL changes.
- One `#` H1 per page (usually the page title); use `##`/`###` below it.
- Keep page `title` values unique — `parent` matching depends on them.
- Do not edit `_site/`, `.jekyll-cache/`, or `Gemfile.lock`.

## Configuration notes

- `_config.yml`:
  - `url: "https://jackneer.github.io"` and `baseurl: ""` — **do not change**.
    A GitHub Pages *user site* (`username.github.io`) must keep `baseurl` empty.
  - `remote_theme` — see the pinning warning above.
  - `search_enabled: true`, `heading_anchors: true`, `color_scheme: light`.
  - `exclude:` — keeps meta files (`AGENTS.md`, `README.md`, `LICENSE`, tooling)
    out of the published output. Add new non-content files here too.
- To change the theme version, edit `remote_theme` only; no vendored theme files exist.

## Local development (optional)

GitHub Pages builds in CI, so local tooling is optional. To preview locally:

```bash
gem install bundler
# create a Gemfile with: gem "github-pages", group: :jekyll_plugins
bundle install
bundle exec jekyll serve   # http://localhost:4000
```

## Publishing

1. Make the change on `main` (or on `draft` then merge).
2. Push. The **Deploy site to GitHub Pages** workflow runs automatically.
3. Watch it under the repo's **Actions** tab.
4. Verify at https://jackneer.github.io.

- **Manual deploy:** Actions → *Deploy site to GitHub Pages* → **Run workflow**.
- **Rollback:** revert the offending commit on `main` and push; the site redeploys.

### Deploy pipeline (`.github/workflows/pages.yml`)

Trigger: `push` to `main`, plus `workflow_dispatch`.
Jobs: `build` (checkout → `configure-pages` → `jekyll-build-pages` →
`upload-pages-artifact`) then `deploy` (`deploy-pages`).
Permissions: `contents: read`, `pages: write`, `id-token: write`.

## Guardrails for agents

- **Never commit secrets, tokens, or credentials.** Do not print token values in logs.
- **Do not change** `url`, `baseurl`, or the Pages `build_type` unless explicitly asked.
- **Do not delete or rewrite content** the user did not ask you to change.
- **Verify before claiming success:** after a change, confirm the workflow succeeded and
  the affected URLs return `200`.
- Prefer targeted edits over full-file rewrites; preserve existing front matter.
- Keep `main` and `draft` consistent when both are in use.

## Verification checklist

After any content/config change:

- [ ] The *Deploy site to GitHub Pages* workflow completed with `success`.
- [ ] Home page returns `200`: https://jackneer.github.io/
- [ ] New/edited pages return `200` at their expected URLs.
- [ ] Theme assets load (`/assets/css/just-the-docs-default.css`, `/assets/js/just-the-docs.js`).
- [ ] Sidebar navigation and `nav_order` look correct.
- [ ] `main` and `draft` are in sync (if `draft` is in use).
