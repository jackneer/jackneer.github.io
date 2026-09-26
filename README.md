# jackneer.github.io — "Jack Wu's Docs"

A personal knowledge base and documentation site, published with **GitHub Pages**
and built with **Jekyll** using the [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/) theme.

> **Status: LIVE** — published at **https://jackneer.github.io**

---

## Repository layout

| Path | What it is |
|---|---|
| `_config.yml` | Site configuration (title, theme, skin, plugins, defaults) |
| `_data/navigation.yml` | Sidebar and top-bar navigation |
| `index.md` | Home page |
| `docs/` | Knowledge base content (Markdown) |
| `about.md` | About page |
| `.github/workflows/pages.yml` | Deployment pipeline (runs on every push to `main`) |
| `LICENSE` | MIT license |

## Branches

| Branch | Purpose |
|---|---|
| `main` | **Live site.** Every push to `main` triggers a build + deploy to GitHub Pages. |
| `draft` | Optional staging branch. Edit here and merge into `main` when you want to publish a change. |

Either edit directly on `main` (fastest), or work on `draft` and merge to `main` when ready.

## Authoring content

1. Write or edit a Markdown file under `docs/` (or add a new one).
2. Give every page a front-matter block at the top:

   ```yaml
   ---
   title: Title shown at the top of the page
   permalink: /docs/my-page/
   excerpt: "One-line summary."
   ---
   ```

3. Add the page to `_data/navigation.yml` so it shows up in the sidebar:

   ```yaml
   docs:
     - title: "Getting Started"
       children:
         - title: "My page"
           url: /docs/my-page/
   ```

   This step is easy to forget — without it the page is live at its URL but
   invisible in the navigation.

4. Commit and push. The GitHub Actions workflow rebuilds and redeploys automatically.

See [docs/writing-pages.md](docs/writing-pages.md) for the full guide.

## Previewing the site

- **Live:** https://jackneer.github.io
- **Locally (optional):** install Ruby and the Jekyll GitHub Pages gems, then run
  `bundle exec jekyll serve` and open http://localhost:4000.

## How deployment works

- **Settings → Pages → Source: GitHub Actions**.
- Pushes to `main` run `.github/workflows/pages.yml`:
  checkout → `configure-pages` → `jekyll-build-pages` → `upload-pages-artifact` → `deploy-pages`.
- You can also trigger it manually from the **Actions** tab (`workflow_dispatch`).

Details: [docs/publishing.md](docs/publishing.md).
