# steff-ml.github.io

The source for my personal website: one data product, written up as a build log,
plus short concept pages and notes. Built with [Jekyll](https://jekyllrb.com/)
using the [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/)
theme, and deployed automatically to GitHub Pages.

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
Markdown files (like this one), applies the layouts, and outputs finished
HTML. **Minimal Mistakes** is the theme — it controls how the site looks and
provides the base layouts.

---

## How the site is structured

```
Home: intro + product card + concept areas + "Follow along"
│
├── Product page: problem · proposed solution · build log
│     └── Build log: chapters (and sections) → posts, in order
│
├── Concepts: one short, timeless page per idea, grouped in areas
│
└── Notes: TILs and monthly link roundups
```

There are two kinds of pages:

| Kind | What | Date | Updated later? |
|---|---|---|---|
| **Timeless** | Home, the product page, concept pages | "Last updated" only | Yes |
| **Dated** | Build-log posts, TILs, link roundups | Yes | No, at most a dated correction |

**The layouts do the linking.** A post only says which chapter it belongs to
and which concepts it uses. The site then adds the line above the title,
previous / build log / next at the bottom, the post's place in the build log,
and "Used in" on every concept page it names. Don't write those links by hand.

---

## Repository layout

| Path | What it's for |
|---|---|
| `index.md` | The home page. The intro text is the Markdown block at the top. |
| `_products/` | Product pages. Currently one: `from-question-to-data-product.md`. |
| `_pages/products.md` | The `/products/` list page. |
| `_posts/build-log/` | Build-log posts. One file per post. |
| `_posts/til/` | TILs. |
| `_posts/links/` | Monthly link roundups. |
| `_concepts/` | Concept pages. One file per concept. |
| `_pages/concepts/` | The `/concepts/` overview and one page per concept area. |
| `_pages/notes.md` | The `/notes/` page (lists TILs and link roundups). |
| `_data/series.yml` | The build log's chapters and sections, and which product page shows it. |
| `_data/concepts.yml` | The concept areas, plus a private list of planned concepts (never shown). |
| `_data/navigation.yml` | The top navigation menu. |
| `_layouts/` | `single.html` (posts), `concept.html`, `concept-area.html`, `product.html`. |
| `_includes/` | `series-tree.html` (the build log), `latest.html` (spare: a dated stream). |
| `assets/images/posts/` | Post cartoons. |
| `assets/cv/` | The CV linked from the home page banner. |
| `_archive/` | Old pages and posts. Not built, kept for later retrieval. |
| `_config.yml` | Site-wide settings: title, author bio, social links, plugins. Edit rarely. |
| `.github/workflows/deploy.yml` | The build-and-deploy automation. Don't touch unless changing the pipeline. |
| `Gemfile` | Lists the Ruby packages (Jekyll + plugins) the site needs. |

Folders starting with `_` are special to Jekyll — it processes them rather than
copying them as-is. `_archive/` is excluded from the build in `_config.yml`.

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

## What to write, and where it goes

The templates live in the strategy repo:
`strategy-foundation/blog/sjablonen/`. Each one explains its fields in a
comment at the top; delete the comment when you're done.

| I want to write… | Template | Save as | URL |
|---|---|---|---|
| A build-log post | `post.md` | `_posts/build-log/YYYY-MM-DD-short-title.md` | `/build-log/short-title/` |
| A TIL | `til.md` | `_posts/til/YYYY-MM-DD-topic.md` | `/til/topic/` |
| A link roundup | `links.md` | `_posts/links/YYYY-MM-DD-month-year.md` | `/links/month-year/` |
| A concept page | `begrip.md` | `_concepts/short-name.md` | `/concepts/short-name/` |

### Build-log posts

The front matter fields that place a post:

```yaml
categories: [build-log]
chapter: stack        # a chapter or section slug from _data/series.yml
part: 1               # order within that chapter or section
image: /assets/images/posts/components.png   # the cartoon at the top
image_alt: "What the cartoon shows"
concepts:
  - gall-s-law        # concept file names, without .md
```

Chapter and section slugs (from `_data/series.yml`):

| Slug | Chapter / section |
|---|---|
| `stack` | I. Building an independent AI stack |
| `wall` | II. Designing an automated data discovery tool → Hitting a wall |
| `method` | II. … → Developing a general method for reliable AI systems design |
| `migrate` | II. … → Migrate a working AI system to your independent stack |
| `pretotyping` | III. Automated pretotyping |
| `prototyping` | IV. Automated prototyping |
| `build` | V. Automated build |

Chapter II has sections, so its posts use a section slug (`wall`, `method`,
`migrate`), not `discovery`. Parts are numbered within each chapter or section,
starting at 1.

**Cartoons:** put the image in `assets/images/posts/` and point `image:` at it.
Without `image:`, a build-log post shows a dashed placeholder, so you can see
which ones still need a cartoon. TILs and link roundups don't get a placeholder.

### Concept pages

- `group:` picks the area: `ai`, `systems`, `data` or `bio`.
- The file name is the concept's name everywhere else: `_concepts/gall-s-law.md`
  is `gall-s-law` in a post's `concepts:` list.
- Change `last_modified_at` whenever you update the page.
- A concept only shows on the site once its page exists. A name in a post's
  `concepts:` without a page is skipped silently, so write the concept first.
- An area (and its chip on the home page) appears as soon as it has one
  concept.

### Changing the build log

Chapters, sections, titles and statuses (`done`, `turning-point`, `ongoing`,
`later`) are in `_data/series.yml`. The `product:` key there names the product
page that shows the build log (`product:`).

### Two things that trip everyone up

1. **A post dated in the future won't appear.** Jekyll silently skips posts whose
   date is later than the build time. If a new post doesn't show up, check the
   date in both the filename and the front matter.
2. **Drafts** go in `_drafts/` with *no date* in the filename — e.g.
   `_drafts/half-finished-idea.md`. Preview them with
   `bundle exec jekyll serve --drafts`. They won't publish until moved into
   `_posts/` with a proper dated filename.

---

## How to add other images

1. Drop the image file into `assets/images/`, e.g.
   `assets/images/my-diagram.png`.
2. Reference it with a leading slash:

```markdown
![A description of the image for accessibility](/assets/images/my-diagram.png)
```

Plain Markdown images in a post are capped at 420 px wide. For a screenshot or
diagram that should use the full width, with a small caption below, use a figure:

```html
<figure class="post-figure">
  <img src="{{ '/assets/images/posts/my-screenshot.png' | relative_url }}" alt="What the image shows">
  <figcaption>One sentence about what the reader should notice.</figcaption>
</figure>
```

Keep images reasonably sized (compress large screenshots) — the whole published
site has a 1 GB ceiling, and oversized media is the fastest way to bloat it.

---

## How to edit the home page

`index.md`. The intro is plain Markdown in the `{% capture intro %}` block at
the top of the file. The banner on the left (photo, tagline, Email · GitHub ·
LinkedIn, CV button) is the HTML just below it. The product card and the
concept chips fill themselves. The "Follow along" boxes link to the RSS feed
(`/feed.xml`, generated by `jekyll-feed`) and to an email with a preset subject.

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
  - title: "Product"
    url: /products/from-question-to-data-product/
  - title: "Concepts"
    url: /concepts/
  - title: "Notes"
    url: /notes/
```

The site title in the top left links to the home page. Add or reorder entries
here to change the menu.

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
| Post isn't in the build log | `chapter:` doesn't match a slug in `_data/series.yml`, or `part:` is missing. |
| Concept doesn't show on a post | No file in `_concepts/` with that exact name. |
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
