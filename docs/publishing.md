---
title: Publishing
nav_order: 3
---

# Publishing

The site is **deliberately unpublished** until you say go. Everything needed to go
live is wired up, but nothing is triggered from a `draft` branch.

## What is "live" vs "in the repo"

| | GitHub Pages |
|---|---|
| `draft` branch | Site source — safe, never deployed |
| `main` branch  | Deployment is armed via GitHub Actions and only runs on pushes to `main` |
| Pages setting   | Must be turned on (Source: GitHub Actions) before any deploy succeeds |

## Go-live checklist (do this only when you are ready)

1. GitHub → **Settings → Pages → Source: GitHub Actions**.
2. Merge `draft` into `main`, or push the branch contents to `main`.
3. The **Deploy site to GitHub Pages** workflow builds and publishes the site.
4. Your site is live at **https://jackneer.github.io**.

## Roll back

Publishing is just a branch + a GitHub setting, so rolling back is easy: push an
earlier version to `main`, or turn GitHub Pages off again in Settings.
