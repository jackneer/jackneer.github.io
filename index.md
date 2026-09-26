---
layout: splash
title: Home
permalink: /
header:
  overlay_color: "#4d6a8c"
  overlay_filter: "0.4"
excerpt: >-
  A personal knowledge base, written in Markdown and published with GitHub Pages.
intro:
  - excerpt: >-
      Every page here is a plain Markdown file in a public repository. There is no
      database and no build tooling to install — GitHub Actions builds the site in
      the cloud on every push.
feature_row:
  - title: "Introduction"
    excerpt: "What this site is and what lives in it."
    url: /docs/introduction/
    btn_label: "Read"
    btn_class: "btn--primary"
  - title: "Writing pages"
    excerpt: "How to add or edit content, and how navigation works."
    url: /docs/writing-pages/
    btn_label: "Read"
    btn_class: "btn--primary"
  - title: "Publishing"
    excerpt: "How a change gets from a commit to the live site."
    url: /docs/publishing/
    btn_label: "Read"
    btn_class: "btn--primary"
---

{% include feature_row id="intro" type="center" %}

{% include feature_row %}
