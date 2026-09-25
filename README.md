# jackneer.github.io — "Jack Wu's Docs"

A personal knowledge base and documentation site, published with **GitHub Pages**
and built with **Jekyll** using the [Just the Docs](https://just-the-docs.github.io/just-the-docs/) theme.

> **Status: under construction — the site is NOT published yet.**
> It lives on the `draft` branch, on purpose, so nothing can go live by accident.

---

## Repository layout

| Path | What it is |
|---|---|
| `_config.yml` | Site configuration (title, theme, search, footer) |
| `index.md` | Home page |
| `docs/` | Knowledge base content (Markdown) |
| `about.md` | About page |
| `.github/workflows/pages.yml` | Deployment pipeline (armed; only fires on pushes to `main`) |
| `LICENSE` | MIT license |

## Authoring content

1. Write or edit a Markdown file under `docs/` (or add a new one).
2. Give every page a front-matter block at the top:

   ```yaml
   ---
   title: Title shown in the sidebar
   nav_order: 5
   # Sections: add "has_children: true" here, and give child pages
   # a matching "parent: Section Title".
   ---
   ```

3. Commit to `draft` and push. That's it — no build steps on your machine.

See [docs/writing-pages.md](docs/writing-pages.md) for the full guide.

## Previewing the site

The site is not published yet, so there is no live URL. To preview:

- **Locally (optional):** install Ruby and the Jekyll GitHub Pages gems, then run
  `bundle exec jekyll serve` and open http://localhost:4000.
- **Via the deploy pipeline:** actually merging to `main` publishes — don't do
  that until you're ready (see below).

## Publishing checklist (do this only when you say go)

1. GitHub → **Settings → Pages → Source: GitHub Actions**.
2. Merge `draft` into `main` (or push the branch contents to `main`).
3. The **Deploy site to GitHub Pages** workflow builds and deploys the site.
4. The site goes live at **https://jackneer.github.io**.
5. (Optional later) Connect a custom domain via **Settings → Pages → Custom domain**.

Full details: [docs/publishing.md](docs/publishing.md).
