# steff-ml.github.io

The source for my personal website — portfolio, blog, and (eventually) podcast.
Built with [Jekyll](https://jekyllrb.com/) using the
[Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/) theme, and
deployed automatically to GitHub Pages.

**Live at:** https://steff-ml.github.io
*(will become my custom domain once set up)*

---

## How this site works (the mental model)

There is no server, no database, and no admin panel. The whole site is a folder
of text files. Here's the chain of events from "I write something" to "it's
online":

```
I edit a Markdown file  →  git commit + git push
        ↓
GitHub sees the push and runs a build (GitHub Actions)
        ↓
Jekyll turns my Markdown + theme into a plain HTML website
        ↓
GitHub Pages serves that HTML to visitors
```

Every push to the `main` branch triggers this automatically. A build takes
roughly 1–2 minutes, after which the change is live. I never upload files to a
server myself — pushing to GitHub *is* publishing.

**Jekyll** is the engine that converts my content into a website. It reads
Markdown files (like this one), applies the theme's layouts, and outputs finished
HTML. **Minimal Mistakes** is the theme — it controls how the site looks and
provides the layouts (blog post, about page, author sidebar, etc.).

---

## Repository layout

The files and folders that matter day to day:

| Path | What it's for |
|---|---|
| `_config.yml` | Site-wide settings: title, author bio, social links, plugins. Edit rarely. |
| `_posts/` | Blog posts live here. One file per post. |
| `_pages/` | Standalone pages (about, etc.). |
| `about.md` | The about page content. |
| `assets/images/` | Images used across the site (bio photo, post images). |
| `_data/navigation.yml` | The top navigation menu. |
| `.github/workflows/deploy.yml` | The build-and-deploy automation. Don't touch unless changing the pipeline. |
| `Gemfile` | Lists the Ruby packages (Jekyll + plugins) the site needs. |

Folders starting with `_` are special to Jekyll — it processes them rather than
copying them as-is.

---

## One-time setup on a new computer

If I ever clone this repo fresh (new machine, etc.), here's how to get it
running locally. Assumes WSL / Ubuntu.

```bash
# Install Ruby and Jekyll's dependency manager (once per machine)
sudo apt update
sudo apt install ruby-full build-essential zlib1g-dev
gem install bundler

# Clone and enter the repo
git clone https://github.com/steff-ml/steff-ml.github.io.git
cd steff-ml.github.io

# Install the site's specific dependencies
bundle install
```

> **WSL speed note:** Jekyll is slow when the project lives on the Windows drive
> (`/mnt/c/...`) and live-reload often won't auto-refresh there. For comfortable
> local work, keep the repo in the Linux home folder (`~/steff-ml.github.io`)
> instead.

---

## The everyday workflow

Whatever the change — new post, edited page, config tweak — the loop is the same:

```bash
# 1. (Optional but recommended) preview locally first
bundle exec jekyll serve --livereload
#    → open http://localhost:4000, edits refresh as you save
#    → press Ctrl+C to stop the preview server

# 2. Publish
git add .
git commit -m "Describe what changed"
git push
```

About two minutes after the push, the change is live. I can watch the build on
the repo's **Actions** tab — green check means deployed, red means something
broke (see Troubleshooting).

---

## How to write a new blog post

Blog posts are Markdown files in the `_posts/` folder. **The filename format is
mandatory** and Jekyll reads the date from it:

```
_posts/YYYY-MM-DD-a-short-slug.md
```

For example: `_posts/2026-07-25-first-week-with-jekyll.md`

Every post starts with a **front matter** block — the section between the two
`---` lines. This is metadata Jekyll reads; the actual post goes below it:

```markdown
---
title: "My Post Title"
date: 2026-07-25 10:00:00 +0200
categories: [engineering]
tags: [jekyll, data]
excerpt: "One sentence shown in the post list and social previews."
---

The body of my post starts here, in normal Markdown.

## A subheading

A paragraph. **Bold**, *italic*, and [a link](https://example.com) all work.

- A bullet
- Another bullet

Code blocks work too:

​```python
print("hello")
​```
```

Two things that trip everyone up:

1. **A post dated in the future won't appear.** Jekyll silently skips posts whose
   date is later than the build time. If a new post doesn't show up, check the
   date in both the filename and the front matter.
2. **Drafts** go in a `_drafts/` folder (create it if needed) with *no date* in
   the filename — e.g. `_drafts/half-finished-idea.md`. Preview them with
   `bundle exec jekyll serve --drafts`. They won't publish until moved into
   `_posts/` with a proper dated filename.

---

## How to add an image to a post

1. Drop the image file into `assets/images/`, e.g.
   `assets/images/my-diagram.png`.
2. Reference it in the post with a leading slash:

```markdown
![A description of the image for accessibility](/assets/images/my-diagram.png)
```

Keep images reasonably sized (compress large screenshots) — the whole published
site has a 1 GB ceiling, and oversized media is the fastest way to bloat it.

---

## How to edit the About page

The about content is in `about.md` (or `_pages/about.md` depending on how it's
organized). Edit the Markdown below the front matter block, save, commit, push.
The `permalink:` line in its front matter controls its URL (`/about/`).

---

## How to change site-wide settings

`_config.yml` holds everything global. Common edits:

- **Bio, name, avatar** → the `author:` section.
- **Social links in the sidebar** → the `author: links:` list.
- **Footer links** → the `footer: links:` list.
- **Site title / description** → the top `title:` and `description:` keys.

> **Important:** `_config.yml` is the *one* file that does **not** hot-reload.
> After editing it, stop the local preview (Ctrl+C) and run
> `bundle exec jekyll serve` again to see the change.

Settings currently worth keeping accurate: `github_username` should be
`steff-ml`, and any placeholder social links (bare `twitter.com/`, etc.) should
point at real profiles or be removed.

---

## How to change the navigation menu

The top menu is defined in `_data/navigation.yml`:

```yaml
main:
  - title: "About"
    url: /about/
  - title: "Blog"
    url: /blog/
  - title: "Podcast"
    url: /podcast/
```

Add or reorder entries here to change the menu.

---

## Publishing / authentication notes

Pushing to GitHub uses a **Personal Access Token** (PAT), not a password —
GitHub disabled password auth for Git. If a push ever asks for credentials:

- **Username:** `steff-ml`
- **Password:** paste the PAT (from GitHub → Settings → Developer settings →
  Personal access tokens). It needs the **`repo`** and **`workflow`** scopes —
  the `workflow` scope is required because this repo contains a workflow file.

The token is cached locally (`git config --global credential.helper store`), so
this normally only comes up on a new machine or after a token expires.

---

## Deployment details (for reference)

Deployment is handled by `.github/workflows/deploy.yml` via GitHub Actions, **not**
by GitHub's older built-in Jekyll builder. This matters because it lets the site
use Jekyll 4 and the Minimal Mistakes theme gem without plugin restrictions.

For this to work, the repo's **Settings → Pages → Source** must be set to
**"GitHub Actions"** (not "Deploy from a branch"). If deploys ever start failing
with a theme-not-found error mentioning Jekyll 3.x, that setting has reverted —
switch it back.

---

## Troubleshooting

| Symptom | Likely cause & fix |
|---|---|
| Pushed, but site didn't change | Check the **Actions** tab — the build may have failed (red X). Click it for the error. |
| Build fails: "theme could not be found" (Jekyll 3.x in log) | Pages Source is on "Deploy from a branch" instead of "GitHub Actions". Fix in Settings → Pages. |
| Build fails: `cannot load such file -- webrick` | The `webrick` gem is missing from the `Gemfile`. Add `gem "webrick", "~> 1.8"`. |
| New post doesn't appear | Filename isn't `YYYY-MM-DD-slug.md`, or the date is in the future. |
| Local preview won't start | Run `bundle install` first; make sure `webrick` is in the Gemfile. |
| Local preview slow / won't auto-refresh | Repo is on `/mnt/c/...`; move it to `~/` in WSL. |
| CSS missing / links broken | `baseurl` in `_config.yml` is wrong — should be `""` for this repo. |
| Push rejected mentioning "workflow scope" | The PAT lacks the `workflow` scope. Regenerate it with that box ticked. |

---

## Quick reference

```bash
# Preview locally
bundle exec jekyll serve --livereload

# Preview including drafts
bundle exec jekyll serve --drafts

# Publish any change
git add . && git commit -m "message" && git push

# After editing _config.yml, restart the preview server (it doesn't hot-reload)
```