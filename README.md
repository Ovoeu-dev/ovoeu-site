# Ovoeu Website

Single-page site for Ovoeu, a bespoke lab-grown jewelry atelier in Oklahoma City.
Six routes (Home, Editions, Book, FAQ, Contact, Legal) behind tabbed navigation.
No build step, no dependencies to install — plain static files.

## Live site

Served by GitHub Pages from the `/docs` folder on `main`.

## Repo layout

```
Ovoeu Site.dc.html    working source — edit this
support.js            runtime (do not hand-edit)
assets/               wordmark, icon, illustration
docs/                 what GitHub Pages serves
  index.html          copy of Ovoeu Site.dc.html
  support.js
  assets/
  .nojekyll
```

## Making a change

1. Edit `Ovoeu Site.dc.html`.
2. Copy it over the published copy:

```sh
cp "Ovoeu Site.dc.html" docs/index.html
cp support.js docs/support.js
cp -R assets/. docs/assets/
```

3. Commit and push. Pages redeploys in about a minute.

Open either file directly in a browser to preview — no server needed.

## First-time Pages setup

1. Push this repo to GitHub.
2. Settings → Pages → Source: **Deploy from a branch**.
3. Branch: `main`, folder: `/docs`. Save.
4. The URL appears at the top of that page once the first deploy finishes.

For a custom domain, add it under Settings → Pages → Custom domain, then point a
CNAME at `<owner>.github.io` at your DNS provider. Pages writes a `CNAME` file
into `/docs` — leave it there.

## Notes

React and Babel load from unpkg at runtime, so the published site needs an
internet connection. Everything else is local to the repo.
