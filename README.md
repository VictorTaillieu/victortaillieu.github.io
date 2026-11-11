# victortaillieu.github.io

Personal website and portfolio — <https://victortaillieu.github.io>

Built with [Hugo](https://gohugo.io/) and the [Blowfish](https://blowfish.page/) theme, deployed to GitHub Pages by GitHub Actions.

## Requirements

| Tool | Version | Why |
|---|---|---|
| [Hugo **extended**](https://github.com/gohugoio/hugo/releases) | `0.167.0` (same as CI) | Builds the site |
| [Go](https://go.dev/dl/) | any recent version | Downloads the theme (Hugo Modules) |
| Git | — | — |

On Ubuntu:

```bash
# Hugo: use the official .deb to get the exact version (apt/snap ship other versions)
wget https://github.com/gohugoio/hugo/releases/download/v0.167.0/hugo_extended_0.167.0_linux-amd64.deb
sudo apt install ./hugo_extended_0.167.0_linux-amd64.deb

sudo apt install golang-go
```

## Run locally

```bash
git clone git@github.com:VictorTaillieu/victortaillieu.github.io.git
cd victortaillieu.github.io
hugo server          # http://localhost:1313, live reload
hugo server -D       # also show drafts
```

The theme is downloaded automatically on the first run (cached in `~/.cache/hugo_cache`).

> Only one Hugo process can run per project: stop `hugo server` before running `hugo` (build lock `.hugo_build.lock`).

## Project structure

```text
.
├── archetypes/            # Templates used by `hugo new`
├── assets/img/            # Images processed by Hugo (author photo)
├── config/_default/
│   ├── hugo.toml          # Site settings (URL, taxonomies, sitemap, outputs)
│   ├── params.toml        # Blowfish options — only values that differ from the theme defaults
│   ├── languages.en.toml  # English: title, author, social links
│   ├── languages.fr.toml  # French: disabled until content is translated
│   ├── menus.en.toml      # Header menu (EN)
│   ├── menus.fr.toml      # Header menu (FR)
│   ├── markup.toml        # Markdown rendering, code highlighting, table of contents
│   └── module.toml        # Theme import (Hugo Module)
├── content/
│   ├── _index.md          # Homepage ("About me" text)
│   ├── cv/                # CV page (timeline) + attached PDFs
│   ├── posts/             # Blog posts, one folder per post: YYYY-MM-slug/index.md + images
│   └── projects/          # Projects, one folder per project: slug/index.md + featured.png
├── layouts/shortcodes/    # Overrides of theme templates (timelineItem)
├── static/                # Files copied as-is to the site root (favicons, manifest)
├── go.mod / go.sum        # Pinned theme version
└── .github/workflows/     # Build & deploy to GitHub Pages
```

Content conventions:

- Each post/project is a **page bundle**: a folder with `index.md` and its images next to it.
- `featured.png` (or `.jpg`) in a bundle is used as thumbnail and hero image.
- Front matter is YAML (`---`). Posts use `title`, `summary`, `date`, `tags`; projects use `title`, `summary`, `weight` (order on the Projects page).

## Write content

```bash
hugo new content posts/2026-11-my-new-post/index.md
hugo new content projects/my-project/index.md
```

New content is created with `draft: true`: remove it (or set `false`) to publish.

### Translations (French)

A translation is a file next to the original with the language code: `index.md` → `index.fr.md` (or `_index.fr.md` for sections). Hugo links both versions automatically; untranslated pages simply don't exist in French.

Once enough content is translated, set `disabled = false` in `config/_default/languages.fr.toml`. English stays at the root (`/`), French is served under `/fr/`.

## Deploy

Push to `master`: the [workflow](.github/workflows/gh-pages.yml) builds the site with `hugo --gc --minify` and deploys it to GitHub Pages. It can also be run manually from the Actions tab (*Run workflow*).

If the build fails, the previous version stays online. Check the run logs in the **Actions** tab.

## Maintenance

### Update the theme

```bash
hugo mod get -u github.com/nunocoracao/blowfish/v3   # latest v3.x release
hugo mod tidy
hugo server                                           # check that everything still looks right
```

Before updating:

1. Read the [Blowfish release notes](https://github.com/nunocoracao/blowfish/releases) for changed options.
2. Check the **supported Hugo range** of the new release (`[module.hugoVersion]` in the theme's `config.toml`). The site does not build reliably outside of it.

A new **major** version (`v4`) requires changing the import path in `config/_default/module.toml` and `go.mod`.

### Update Hugo

1. Pick a version within the theme's supported range.
2. Install it locally (see [Requirements](#requirements)).
3. Update `hugo-version` in `.github/workflows/gh-pages.yml` **and** the version in this README.

### Theme overrides

`layouts/shortcodes/timelineItem.html` overrides the theme's shortcode of the same name. After a theme update, compare it with the theme's version (in `~/.cache/hugo_cache/modules/.../blowfish/v3@<version>/layouts/shortcodes/`) to pick up fixes.
