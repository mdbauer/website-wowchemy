# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build and Development Commands

```bash
# Local development server (requires Hugo v0.108.0)
hugo server

# Build for production
hugo --gc --minify

# Build with draft/future content
hugo --gc --minify --buildFuture
```

The site is deployed via Netlify. Pushing to master triggers automatic builds. Deploy previews build with `--buildFuture` to include draft/future-dated content.

## Architecture

This is a Hugo static site using the Wowchemy/Academic theme (v5.7.1) for an academic personal website (michaeldbauer.com). No custom layouts exist — all customization is done through configuration, frontmatter, and SCSS overrides.

### Directory Structure

```
config/_default/          # Site configuration
  config.yaml             #   Base settings, module imports, permalinks, taxonomies
  params.yaml             #   Theme, appearance, contact info, feature toggles
  menus.yaml              #   Navigation menu (Home, Research, Other Pubs, Teaching, CV, Contact)
  languages.yaml          #   Single-language (en-us)
content/
  home/                   #   Homepage widgets (about, news, currentwork, seminars, disclaimer)
  publication/            #   ~25 research publications, each in its own folder
  research/               #   Research page: widget_page with published/working paper/WIP sections
  authors/admin/          #   Author profile (name, bio, affiliations, education, social links)
  contact/                #   Contact page with Netlify form
  other-pubs.md           #   Non-peer-reviewed articles, blog posts, SF Fed Economic Letters
  teaching.md             #   Course listings with syllabus links
  post/                   #   Blog posts (mostly unused)
  slides/                 #   Reveal.js slides (example only)
assets/scss/custom.scss   #   Minimal style overrides (container width, heading sizes, font weights)
static/files/             #   PDFs, datasets (.csv, .xlsx), replication packages (.zip), syllabi
data/                     #   page_sharer.toml, fonts/, themes/
netlify.toml              #   Build config (Hugo 0.108.0, gc, minify, resource caching plugin)
go.mod / go.sum           #   Hugo module dependencies (Wowchemy v5.7.1, plugins)
```

### Content Patterns

**Publications** use this structure:
```
content/publication/{slug}/
  index.md     # Frontmatter with title, authors, date, abstract, links
  cite.bib     # BibTeX citation
```

Publication frontmatter fields:
- `publication_types`: `"2"` = journal articles (peer-reviewed), `"3"` = working papers
- `featured: true/false` — highlights key papers
- `links`: array of `{name, url}` objects. Common link names: "Article", "PDF", "Working Paper", "Online Appendix", "Code & Data", "FRBSF Working Paper", "SSRN Working Paper"
- PDFs and data stored in `/static/files/` and referenced as `files/filename.ext`
- Two frontmatter formats exist: newer files use YAML (`---`), some older files use TOML (`+++`)

**Homepage widgets** are markdown files in `content/home/` with `widget` type and `weight` controlling display order:
| Weight | File | Widget | Description |
|--------|------|--------|-------------|
| 20 | about.md | about | Author profile card |
| 20 | news.md | blank | News and announcements |
| 30 | currentwork.md | blank | Current research projects |
| 100 | seminars.md | blank | Seminar series links |
| 200 | disclaimer.md | blank | Views disclaimer |

**Research page** (`content/research/`) is a `widget_page` with three sections:
- `published.md` (weight 50): `pages` widget filtering `publication_type: "2"`, citation view
- `workingpapers.md` (weight 20): `pages` widget filtering `publication_type: "3"`, citation view
- `workinprogress.md` (weight 10): `blank` widget with manually listed projects

### Navigation Menu

Defined in `config/_default/menus.yaml`:
1. Home (`/`)
2. Research (`/research/`)
3. Other Publications (`/other-pubs/`)
4. Teaching (`/teaching/`)
5. CV (`files/cv_mbauer.pdf`)
6. Contact (`/contact/`)

### Configuration Details

- **Themes**: Day = "ocean", Night = "minimal", Font = "ocean" size L
- **Hugo modules**: Wowchemy v5.7.1 + netlify, netlify-cms, reveal plugins
- **Taxonomies**: tags, categories, publication_types, authors
- **Enabled**: RSS, emoji, syntax highlighting (R), robots.txt, reading time, related content, search (Wowchemy), maps (Mapnik)
- **Disabled**: Math rendering, diagrams, comments, analytics, page edit links

### Static Assets

`/static/files/` contains:
- **CV**: `cv_mbauer.pdf`
- **Paper PDFs**: `cpc.pdf`, `green.pdf`, `green_mps.pdf`, `mps.pdf`, `mpu.pdf`, `risk.pdf`, `sdr.pdf`, `trends.pdf`, etc.
- **Online appendices**: `bbm_appendix.pdf`, `br_trends_online_appendix.pdf`, `mpu_online_appendix.pdf`, etc.
- **Datasets**: `mpu.csv`, `isk.xlsx`, `risk_index.xlsx`, `bps_bluechip_rules.xlsx`, `FOMC_Bauer_Swanson.xlsx`
- **Replication packages**: `.zip` files for most published papers
- **Syllabi**: `macro_syllabus.pdf`, `eap_hamburg_syllabus.pdf`, `MFE230E_Syllabus_S2024.pdf`

## Common Tasks

**Adding a new publication:**
1. Create `content/publication/{slug}/index.md` with YAML frontmatter (title, authors, date, publication_types, publication, abstract, links)
2. Add `cite.bib` in the same folder
3. Place any PDF in `static/files/`
4. Use `publication_types: ["2"]` for journal articles or `["3"]` for working papers

**Updating homepage content:**
Edit the relevant widget file in `content/home/` (e.g., `news.md` for announcements, `currentwork.md` for research projects).

**Adding a menu item:**
Add an entry to `config/_default/menus.yaml` under `main:` with `name`, `url`, and `weight`.

**Updating author profile:**
Edit `content/authors/admin/_index.md` (name, bio, interests, education, affiliations, social links).
