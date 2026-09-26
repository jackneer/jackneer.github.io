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
  - title: "My Development Setup"
    excerpt: "A full dev environment on a phone — Termux, PRoot Debian, OpenCode, and a cloud model doing the heavy lifting."
    url: /docs/setup/
    btn_label: "Read"
    btn_class: "btn--primary"
  - title: "Support Triage, With and Without Jev"
    excerpt: "A proof-of-concept: turning a support queue into typed judgments with TypeSafe's Jev model."
    url: /docs/jev-poc/
    btn_label: "Read"
    btn_class: "btn--primary"
  - title: "Guides"
    excerpt: "Longer how-to writeups."
    url: /docs/guides/
    btn_label: "Browse"
    btn_class: "btn--primary"
---

{% include feature_row id="intro" type="center" %}

{% include feature_row %}

## How this site works

- [Introduction]({% link docs/introduction.md %}) — what this site is about
- [Writing pages]({% link docs/writing-pages.md %}) — how to add or edit content, and how navigation works
- [Publishing]({% link docs/publishing.md %}) — how a change gets from a commit to the live site
