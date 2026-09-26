---
title: Writing pages
permalink: /docs/writing-pages/
excerpt: "How to add or edit content, and how the sidebar is built."
redirect_from:
  - /docs/writing-pages.html
---

Every page is a plain Markdown file with a small "front matter" block at the top:

```yaml
---
title: Title shown at the top of the page
permalink: /docs/my-page/
excerpt: "One-line summary, used for previews and search."
---
```

Adding a page takes **two** edits: the Markdown file itself, and an entry in
`_data/navigation.yml`. A page with no navigation entry still builds and is still
reachable by URL, but it will not appear in the sidebar.

## Options you will use most

| Front matter | What it does |
|---|---|
| `title` | Heading shown at the top of the page and in breadcrumbs |
| `permalink` | The URL for the page (always end it with a `/`) |
| `excerpt` | Short summary used in previews and search results |
| `toc` | Show the per-page table of contents (on by default here) |
| `sidebar: nav: "docs"` | Which navigation tree to show (set site-wide already) |
| `redirect_from` | Old URLs that should redirect here |
| `layout` | `single` for normal pages (the default), `splash` for landing pages |

The theme renders `title` as the page's `<h1>`, so do **not** repeat it as a
`#` heading in the body — start at `##` instead.

## Navigation

The sidebar lives in `_data/navigation.yml`. It has two trees:

- `main` — the links in the bar across the top of every page.
- `docs` — the sidebar, a list of groups that each hold `children`.

```yaml
docs:
  - title: "Getting Started"
    children:
      - title: "Introduction"
        url: /docs/introduction/
      - title: "Writing pages"
        url: /docs/writing-pages/
```

Ordering is just the order of the list, so moving a page up the sidebar means
moving its lines up the file. The `url` values must match each page's `permalink`
exactly, including the trailing slash.

To add a group, add another `title` with its own `children`. See
[Guides]({% link docs/guides.md %}) and
[Example page]({% link docs/guides/example-page.md %}) for a working example.

Any Markdown works inside pages: headings, links, lists, code blocks, tables, images.
