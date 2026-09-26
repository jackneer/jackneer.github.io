---
title: Example page
permalink: /docs/guides/example-page/
excerpt: "A minimal example of a page nested under a sidebar group."
redirect_from:
  - /docs/guides/example-page.html
---

This page sits under the *Guides* group in the sidebar. Its front matter is
nothing special:

```yaml
---
title: Example page
permalink: /docs/guides/example-page/
excerpt: "A minimal example of a page nested under a sidebar group."
---
```

The nesting is not declared here — it comes from `_data/navigation.yml`, where
this page is listed as a child of the `Guides` group:

```yaml
  - title: "Guides"
    children:
      - title: "Overview"
        url: /docs/guides/
      - title: "Example page"
        url: /docs/guides/example-page/
```

That means a page's position in the sidebar is independent of where its file
lives on disk. The `docs/guides/` folder is a convention for keeping related
files together, not something the theme reads.
