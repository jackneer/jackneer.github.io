---
title: Guides
permalink: /docs/guides/
excerpt: "Longer how-to writeups."
redirect_from:
  - /docs/guides.html
---

Longer how-to writeups live here. This page is the overview for the *Guides*
group in the sidebar.

Add a guide by:

1. Creating a new Markdown file under `docs/guides/`.
2. Giving it a `title` and a `permalink` (for example `/docs/guides/my-guide/`).
3. Adding it under the `Guides` group's `children` in `_data/navigation.yml`.

Step 3 is the one that is easy to forget — without it the guide builds but never
shows up in the sidebar.

See the example: [Example page]({% link docs/guides/example-page.md %}).
