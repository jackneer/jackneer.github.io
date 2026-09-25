---
title: Example page
parent: Guides
nav_order: 1
---

# Example page

This is a child page. Notice the front matter:

```yaml
---
title: Example page
parent: Guides
nav_order: 1
---
```

- `parent: Guides` matches the `title` of [docs/guides.md]({% link docs/guides.md %})
  which has `has_children: true`.
- `nav_order` orders this page within the *Guides* section.

You can nest deeper: a child page can itself have `has_children: true` and grandchild
pages can point their `parent` at this page's `title`.
