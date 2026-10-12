# Your publication

This repository is your publication. It holds everything about it: your
settings, your author profile, and every article and page you write.
Galascribe reads this repository to build your site and publish it.

## Where articles go

Every article and page lives in `content/`, as a single Markdown file. Each
file starts with a short block of settings (its "frontmatter") between two
`---` lines, followed by your writing:

```markdown
---
schemaId: urn:gala:schema:content-frontmatter:2.0.0
schemaVersion: "2.0.0"
id: <a stable id — do not change once published>
kind: article
title: My new article
language: en-US
authors:
  - <your author id, from gala/authors/>
tags: []
status: published
createdAt: "2026-01-01T00:00:00.000Z"
slug: my-new-article
redirects: []
extensions: {}
---

# My new article

Write your article here, in Markdown.
```

Set `kind` to `article` for a dated piece of writing, or `page` for
something like an About or Contact page that isn't part of your article
feed. `welcome-to-your-publication.md` and `about.md` are two examples
already in `content/` — read them, copy one, and start writing.

## Writing: in Galascribe, or by hand

You can write and edit articles in the Galascribe app, which commits the
changes to this repository for you. You can also edit these files directly
— in any text editor, or by cloning this repository and using `git` — and
push your changes. Both ways work, because everything Galascribe needs is
in the files themselves.

## Your settings

- `gala/publication.json` — your publication's name, description, and base
  address.
- `gala/authors/` — one file per author. Start by editing the file that's
  already there with your own name and short biography.
- `gala/navigation.json` — the links in your site's header and footer.
- `gala/appearance.json` — which visual theme your site uses. New publications
  start with the newsletter-oriented Default theme.
- `gala/modules/interactions.json` — likes and comments are on by default and can be changed in Galascribe under the site's settings or by editing this file.
- `assets/` — images and other files your publication uses that aren't
  articles or pages.

You generally won't need to touch `gala/repository.json` or
`gala.lock.json` by hand — Galascribe manages those.

## Publishing

`.github/workflows/gala-publish-v2.yml` is the file that tells your own
GitHub account how to publish this repository when you're ready. When
Galascribe builds and publishes your site, the work happens inside your own
GitHub account, using GitHub's own build minutes — not on Galascribe's
servers. You don't need to understand this file to use it; just don't
delete or rename it. If Galascribe ever tells you it needs to be updated
(for example, to point at a newer version of the publishing tool), it will
tell you exactly what to change.

## Publishing destinations

- **GitHub Pages** (the default): there is nothing extra to set up. Galascribe
  configures this repository for GitHub Pages when you select the destination;
  publishing then builds and deploys the site.
- **DigitalOcean Spaces**: not offered yet. If you see references to it in
  this repository's files, they are not active for you — Galascribe will
  tell you when this destination becomes available and what you'd need to
  set up for it.

## If something looks broken

If a build fails, check the error Galascribe shows you first — it usually
names the exact file and setting that needs fixing. Nothing here runs code
you wrote or installs anything on your behalf; the tool only reads your
articles, settings, and images, and turns them into a website.
