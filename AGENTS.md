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
| Theme | [`mmistakes/minimal-mistakes`](https://github.com/mmistakes/minimal-mistakes), pinned via `remote_theme` in `_config.yml` |
| Content format | Markdown (`.md`) |
| Deployment | GitHub Actions — `.github/workflows/pages.yml` |
| Pages build type | `workflow` (Actions), **not** legacy branch deploy |

> **Theme pinning:** `remote_theme: mmistakes/minimal-mistakes@<tag>` must point to a
> **tag that actually exists**. Before bumping, confirm it at
> <https://github.com/mmistakes/minimal-mistakes/tags> and that the release still
> supports Jekyll `>= 3.7, < 5.0` (GitHub Pages uses Jekyll 3.10.0). A missing tag makes
> the remote-theme download return `404` and the build fails.

> **Required plugins:** Minimal Mistakes needs `jekyll-paginate`, `jekyll-sitemap`,
> `jekyll-gist`, `jekyll-feed` and `jekyll-include-cache`. All are on the GitHub Pages
> allowlist and are declared in both `plugins` and `whitelist` in `_config.yml`.
> Removing `jekyll-include-cache` breaks the build with an
> `Unknown tag 'include_cached'` error.

## Repository layout

| Path | Purpose |
|---|---|
| `_config.yml` | Site configuration: title, `remote_theme`, skin, plugins, `defaults`, `exclude` |
| `_data/navigation.yml` | **Sidebar and masthead navigation** — a new page is invisible until listed here |
| `index.md` | Home page (`layout: splash`, `permalink: /`) |
| `docs/` | Knowledge base content |
| `docs/guides.md` | Overview page for the *Guides* sidebar group |
| `docs/guides/example-page.md` | Example page nested under that group |
| `docs/jev-poc.md` | Writeup: support triage with TypeSafe's Jev |
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

Adding a page takes **two** edits. Miss the second and the page builds but never
appears in the sidebar.

**1. The Markdown file** needs a front-matter block:

```yaml
---
title: Title shown at the top of the page   # required
permalink: /docs/my-page/                   # required, keep the trailing slash
excerpt: "One-line summary."                # used in previews and search
# toc: false                                # per-page TOC is on by default
# layout: splash                            # landing pages only; default is `single`
# redirect_from: [/docs/my-page.html]       # preserve an old URL
---
```

**2. `_data/navigation.yml`** needs an entry. The `docs` tree is a list of groups,
each with `children`; the `main` tree is the masthead:

```yaml
docs:
  - title: "Getting Started"
    children:
      - title: "My page"
        url: /docs/my-page/
```

### Sections and nesting

- Sidebar structure comes **only** from `_data/navigation.yml`, never from front matter.
- Ordering is list order — to move a page up, move its lines up the file.
- Each `url` must match that page's `permalink` exactly, trailing slash included.
- A page's folder on disk is a filing convention and does not affect the sidebar.
- The theme supports one level of `children` per group; deeper trees are not rendered.

### Conventions

- Put content under `docs/`; use **kebab-case** filenames (`writing-pages.md`).
- Prefer `{% link docs/page.md %}` for internal links so they survive URL changes.
  Liquid does **not** run inside front matter, so `navigation.yml` and keys like
  `feature_row.url` must use literal paths.
- **Do not add a `#` H1** — the theme renders `title` as the page `<h1>`. Start at `##`.
- Keep page `title` values unique.
- Do not edit `_site/`, `.jekyll-cache/`, or `Gemfile.lock`.

## Configuration notes

- `_config.yml`:
  - `url: "https://jackneer.github.io"` and `baseurl: ""` — **do not change**.
    A GitHub Pages *user site* (`username.github.io`) must keep `baseurl` empty.
  - `remote_theme` — see the pinning warnings above.
  - `minimal_mistakes_skin: air`; `search: true`, `breadcrumbs: true`,
    `enable_copy_code_button: true`.
  - `defaults:` — sets `layout: single`, `sidebar.nav: "docs"` and `toc` for all
    pages. **Do not remove it:** Minimal Mistakes has no fallback layout, so
    without this block every page renders unstyled.
  - `plugins:` / `whitelist:` — keep the two lists identical.
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
- [ ] Theme assets load (`/assets/css/main.css`).
- [ ] Sidebar navigation matches `_data/navigation.yml`, and every new page is listed.
- [ ] `main` and `draft` are in sync (if `draft` is in use).
