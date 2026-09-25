---
title: Writing pages
nav_order: 2
---

# Writing pages

Every page is a plain Markdown file with a small "front matter" block at the top:

```yaml
---
title: Title shown in the sidebar
nav_order: 5
---
```

## Options you will use most

| Front matter | What it does |
|---|---|
| `title` | Name shown in the sidebar and page heading |
| `nav_order` | Position in the sidebar (lower = higher up) |
| `has_children: true` | Make this page a section that can hold sub-pages |
| `parent: Section Name` | Put a page under a section (must match a `title` of a page with `has_children: true`) |
| `permalink` | The URL for the page (defaults to the file path) |

## Sections

To create a section with sub-pages, make a page with `has_children: true` and let
its children declare `parent: <Section Title>`. See
[docs/guides.md]({% link docs/guides.md %}) and
[docs/guides/example-page.md]({% link docs/guides/example-page.md %}) for a working example.

Any Markdown works inside pages: headings, links, lists, code blocks, tables, images.
