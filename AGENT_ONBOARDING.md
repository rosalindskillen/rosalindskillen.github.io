# Agent Onboarding & Comprehensive Repository Guide

> **Welcome, Agent / Developer.**  
> This document is the primary, single-source entry point for understanding, navigating, and contributing to the **Rosalind Skillen** website codebase. It assimilates, contextualizes, and connects all previously fragmented notes and recipes in this repository into a unified architectural map.

---

## 1. Repository Identity & Executive Summary

* **Subject / Client:** [Rosalind Skillen](https://www.linkedin.com/in/rosalind-skillen-57b839156/) — Belfast-born, Dublin-based journalist nominated for **Features Journalist of the Year** and **Young Journalist of the Year** at the Irish Journalism Awards 2025.
* **Site Nature:** Static journalism portfolio, broadcast media archive, and personal contact hub.
* **Core Tech Stack:**
  * **Static Site Generator:** [Jekyll 4.3](https://jekyllrb.com/) (Ruby).
  * **Base Theme:** [Jekyll Origin Premium Theme](https://www.zerostatic.io/theme/jekyll-origin/) by Zerostatic Themes (extensively customized).
  * **Hosting & CI/CD:** [Netlify](https://www.netlify.com/) (configured via [`netlify.toml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/netlify.toml)), deployable to GitHub Pages.
  * **Frontend:** Vanilla JavaScript (ES6+), Bootstrap 5 Grid/Reboot, Font Awesome 6, self-hosted web fonts, custom SCSS variables with native CSS custom properties.

---

## 2. Documentation Map: How Existing Notes Connect

Rather than duplicating information across disparate markdown files, use this table to understand what each existing document in the repository covers and when to consult it:

| Document | Primary Focus & Value | When to Consult |
| :--- | :--- | :--- |
| [`docs/jekyll-origin/`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/docs/jekyll-origin/README.md) | **Local copy of the official Zerostatic Jekyll Origin documentation** ([`https://www.zerostatic.io/docs/jekyll-origin/`](https://www.zerostatic.io/docs/jekyll-origin/)), annotated with section-by-section supersession analysis. | **Primary reference** when checking vanilla theme specs vs repo overrides. |
| [`CODE_STRUCTURE.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/CODE_STRUCTURE.md) | High-level summary of core folders and basic content editing instructions. | Quick refresher on Jekyll directory conventions. |
| [`project_overview.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/project_overview.md) | High-level overview of core Jekyll directories and Font Awesome social link instructions. | Quick lookup on editing `_data/social.json`. |
| [`REPOSITORY_BREAKDOWN.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/REPOSITORY_BREAKDOWN.md) | Deep architectural audit, layout matrix, collection inventory, styling pipeline analysis, and cleanup recommendations. | Detailed inventory of every layout, template, and asset in the project. |
| [`JEKYLL_ORIGIN_NOTES_INCONSISTENCIES.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/JEKYLL_ORIGIN_NOTES_INCONSISTENCIES.md) | Critical divergence analysis comparing upstream Zerostatic documentation against this customized repo. | **Essential before assuming theme defaults!** Clarifies path flattenings, `localStorage`, SVG logos, and Formspree usage. |
| [`jekyll-origin-docs-structured-notes.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/jekyll-origin-docs-structured-notes.md) | Upstream vendor reference for the raw Zerostatic Jekyll Origin theme. | Reference when understanding original theme capabilities and unused features. |
| [`media-card-lazy-audio-player-recipe.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/media-card-lazy-audio-player-recipe.md) | Implementation recipe for lazy RTÉ and native audio players within cards. | When adding or debugging audio media entries with timestamp seeking. |
| [`audio-embed-timestamp-recipe.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/audio-embed-timestamp-recipe.md) | Technical guide to media fragment URIs (`#t=seconds`) and iframe postMessage seek protocols. | Deep dive into audio time math and cross-origin player communication. |
| [`image_analysis.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/image_analysis.md) | Audit of active vs. unlinked images across `assets/images/`. | When adding images, auditing asset bloat, or cleaning unused thumbnails. |
| [`todo.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/todo.md) | Short active backlog of requested changes (bio updates, social links, blog link). | Checking pending site enhancements. |
| [`README.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/README.md) | Stock Zerostatic theme readme with generic installation/deploy tips. | Historical vendor reference. |

---

## 3. High-Level Architecture & Information Flow

```
                     ┌───────────────────────┐
                     │     _config.yml       │  Global settings, branding, colors,
                     │  _data/*.yml, *.json  │  navigation, social links, contact
                     └───────────┬───────────┘
                                 │
         ┌───────────────────────┴───────────────────────┐
         ▼                                               ▼
┌───────────────────┐                         ┌───────────────────┐
│      pages/       │                         │   collections/    │
│  Static markdown  │                         │  _posts/ (articles│
│ (home, about,     │                         │  _media/ (TV,     │
│  media, contact)  │                         │   radio, podcasts)│
└────────┬──────────┘                         └─────────┬─────────┘
         │                                               │
         └───────────────────────┬───────────────────────┘
                                 ▼
                     ┌───────────────────────┐
                     │       _layouts/       │  Page structure wrappers:
                     │   default.html        │  (default -> home/media-3/
                     │   home, media-3,      │   basic/post/contact)
                     │   basic, post, etc.   │
                     └───────────┬───────────┘
                                 │
                                 ▼
                     ┌───────────────────────┐
                     │      _includes/       │  Reusable modular components:
                     │  framework/ (nav, seo)│  header, footer, title hero,
                     │  theme/cards/         │  custom interactive card-post
                     └───────────┬───────────┘
                                 │
                                 ▼
                     ┌───────────────────────┐
                     │    _site/ (Output)    │  Static HTML, compiled CSS & JS
                     │  *DO NOT EDIT DIRECTLY│  Overwritten on every build
                     └───────────────────────┘
```

### The Golden Rule of Development in this Repo
> [!CAUTION]
> **Never directly edit files inside `_site/`!**
> `_site/` is Jekyll's compiled output directory. Any changes made directly in `_site/` will be overwritten and lost on the next build. All changes must be made in source files (`pages/`, `collections/`, `_layouts/`, `_includes/`, `_sass/`, `assets/`, `_data/`, `_config.yml`).

---

## 4. Directory & File Inventory

### 4.1 Configuration: [`_config.yml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_config.yml)
Controls build settings and theme variables:
* `collections_dir: collections`: Sets collection root to `collections/`.
* `collections`:
  * `posts`: Output enabled, permalink `/blog/:path/`. Default layout `post`.
  * `media`: Output enabled, permalink `/media/:path/`.
* `colors`: Defines light mode and dark mode hex palettes (accent `#EC255A`).
* `fonts`: Enables self-hosted fonts (`use_self_hosted_fonts: true`) using Schibsted Grotesk (headings/logo), Open Sans (body), and Fira Mono (monospace).
* `contact_form`: Uses Formspree (`use_formspree_form: true`, endpoint configured).
* `darkmode`: Dark mode switcher enabled in header.
* `footer` & `bottom`: Currently disabled (`enable_footer: false`, `enable_bottom: false`).

### 4.2 Data Directory: [`_data/`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_data/)
* [`_data/menu.yml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_data/menu.yml): Defines main navigation links (currently Home `/`, Media `/media/`, About `/about/`, Contact `/contact/`).
* [`_data/social.json`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_data/social.json): Active social profiles (X/Twitter, GitHub, Instagram, LinkedIn, RSS feed) with Font Awesome icon classes.
* [`_data/contact.yml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_data/contact.yml): Contact email address (`rskillen982@icloud.com`).
* [`_data/authors.yml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_data/authors.yml) & [`_data/partners.json`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_data/partners.json): Theme boilerplate files (largely inactive in current layout).

### 4.3 Static Pages: [`pages/`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/pages/)
* [`pages/home.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/pages/home.md) (`/`): Layout [`home`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/home.html). Hero section with headline, description, hero portrait [`assets/images/rosalind-skillen-homepage-hero.png`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/assets/images/rosalind-skillen-homepage-hero.png), social icons, and grid of recent posts.
* [`pages/media.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/pages/media.md) (`/media/`): Layout [`media-3`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/media-3.html). Shows the chronological broadcast/media archive.
* [`pages/about.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/pages/about.md) (`/about/`): Layout [`basic`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/basic.html). Biographical story, beat focus, and speaking availability.
* [`pages/contact.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/pages/contact.md) (`/contact/`): Layout [`contact`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/contact.html). Formspree form with name, email, message, and direct contact email display.
* [`pages/privacy.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/pages/privacy.md) & [`pages/terms.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/pages/terms.md): Legal pages (currently carry generic boilerplate text).
* [`pages/categories.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/pages/categories.md) & [`categories/*.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/categories/): Category index and individual category views.

### 4.4 Collections: [`collections/`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/collections/)
* **`collections/_posts/`**: Written journalism and external articles (*BBC, The Sunday Times, Irish Farmers Journal, Belfast Telegraph, The Currency, The Detail*).
  * Uses `custom_url: "https://..."` to route card clicks directly to external publication websites.
* **`collections/_media/`**: Radio interviews, podcasts, TV segments, and conference speeches (*RTÉ 2FM, Newstalk, Tipp FM, Virgin Media Ireland AM, Beat 102 103, TEDx, Green Foundation Ireland*).
  * Uses custom front-matter fields for lazy inline playback.

### 4.5 Layouts: [`_layouts/`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/)
* [`default.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/default.html): Root document wrapper. Injects meta tags, CSS, inline theme check, header, mobile drawer, main `#wrapper`, scripts, and analytics.
* [`home.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/home.html): Home view template. Combines title include and post cards grid.
* [`media-3.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/media-3.html): **Active media page template.** Renders 2-column full cards sorted reverse chronological by date.
* [`media.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/media.html): Legacy alternative media template (row style).
* [`blog.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/blog.html) & [`blog/index.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/blog/index.html): Paginated portfolio listing (paginated by `jekyll-paginate`).
* [`basic.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/basic.html): Single column content page with header image.
* [`post.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/post.html): Standard single post layout.

### 4.6 Includes: [`_includes/`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_includes/)
* **`framework/`**: Stock components for header ([`header.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_includes/framework/header.html)), mobile menu ([`menu-main-mobile.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_includes/framework/menu-main-mobile.html)), desktop menu ([`menu-main.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_includes/framework/menu-main.html)), hero title ([`title.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_includes/framework/title.html)), social icons ([`social.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_includes/framework/social.html)), contact forms, and SEO tags.
* **`theme/cards/card-post.html`**: **The most important include in the project.** Handles rendering of all post and media cards. Supports interactive players (see Section 5).

### 4.7 Styling System: [`_sass/`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_sass/) & [`assets/css/`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/assets/css/)
The CSS build chain:
```
assets/css/main.scss
  └─ Defines CSS Custom Properties (--color-*, --font-family-*)
  └─ Defines html[data-bs-theme='dark'] custom properties
  └─ @import 'style' (_sass/style.scss)
       ├─ _sass/framework/index.scss (Bootstrap 5, Font Awesome, Zerostatic base)
       └─ _sass/theme/index.scss
            └─ _sass/theme/_custom.scss (Project-specific styling overrides)
```
Key custom styles in [`_sass/theme/_custom.scss`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_sass/theme/_custom.scss):
* Hero title typography and primary color underlines.
* Desktop menu layout and hover styling.
* Card styling: `.card-post`, `.card-row`, `.card-list`, `.card-full`.
* Media player containers: `.video-container` (16:9 responsive aspect ratio) and `.rte-lazy-button` / `.audio-player-active` overlays.

---

## 5. Media Playback Architecture (Interactive Cards)

The repository implements a specialized lazy-loading pattern in [`_includes/theme/cards/card-post.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_includes/theme/cards/card-post.html) for media entries. Instead of loading heavy third-party iframes on page load, cards render lightweight image thumbnails with play overlays. When clicked, the appropriate player is instantiated and auto-seeks.

### Supported Playback Modes

#### Case 1: RTÉ Radio Iframe Player
Used for RTÉ clips where playback requires RTÉ's proprietary Bosco iframe player.
* **Front Matter:**
  ```yaml
  rte_clip_id: "22573616"
  rte_start_seconds: 3331       # Optional seek time in seconds
  rte_start_label: "55:31"       # Optional human label on play button
  ```
* **Mechanism:** On click, injects an `iframe` pointing to `https://www.rte.ie/bosco/components/player/iframe.html?...`. Once loaded, sends `postMessage` calls with `action: "seekto"` and `action: "play"`.

#### Case 2: Direct Native Audio File (GoLoud / Podcasts)
Used when a direct `.mp3` stream URL is accessible.
* **Front Matter:**
  ```yaml
  audio_src: "https://bauernordic-pods.../episode.mp3"
  audio_start_seconds: 104
  audio_start_label: "1:44"
  ```
* **Mechanism:** On click, injects a native HTML5 `<audio controls>` player positioned over the thumbnail with `#t=SECONDS` media fragment and sets `player.currentTime = startAtSeconds`.

#### Case 3: YouTube Video Embed
Used for video interviews and conference keynotes.
* **Front Matter:**
  ```yaml
  youtube_id: "iq52zCD56lw"
  youtube_start_seconds: 2
  youtube_start_label: "0:02"
  ```
* **Mechanism:** On click, injects a responsive 16:9 iframe pointing to `https://www.youtube.com/embed/{id}?autoplay=1&rel=0&start={seconds}`.

#### Case 4: Standard External Link
For text publications without media players:
* **Front Matter:**
  ```yaml
  custom_url: "https://www.irishtimes.com/..." # or link: "..."
  ```
* **Mechanism:** Clicking the thumbnail or title navigates directly to the external article.

---

## 6. Theme Inconsistencies & Stale Defaults

> [!WARNING]
> When maintaining or updating the site, be aware of discrepancies between upstream Zerostatic theme documentation and this actual repo state (detailed in [`JEKYLL_ORIGIN_NOTES_INCONSISTENCIES.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/JEKYLL_ORIGIN_NOTES_INCONSISTENCIES.md)):

1. **Include Directory Structure:** Upstream docs reference nested directories like `_includes/framework/global/head/`. In this repository, includes are flattened directly under `_includes/framework/`.
2. **Dark Mode Storage:** Upstream docs state dark mode uses `sessionStorage`. This repository uses `localStorage` in both [`_layouts/default.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/default.html) and [`assets/js/darkModeSwitch.js`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/assets/js/darkModeSwitch.js), preserving dark mode across browser restarts.
3. **Logos:** Logos are SVGs ([`assets/images/logo/logo.svg`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/assets/images/logo/logo.svg)), not the PNG filenames in the stock theme docs.
4. **Contact Form:** Formspree is actively configured in `_config.yml` (`use_formspree_form: true`), overriding the theme's default Netlify form option.
5. **Stale Demo Metadata in `_config.yml`:**
   * `url: "https://jekyll-origin.netlify.app"` (should eventually be replaced with the real production domain).
   * `meta_twitter_site` and `meta_twitter_creator` still point to `@zerostaticio`.
   * Newsletter Mailchimp form action points to a Zerostatic list.
   * Disqus shortcode points to `"zerostatic"`.
6. **Mismatched Category Files:** The files in `categories/` (`branding.md`, `design.md`, `javascript.md`) are theme stock leftovers. Current posts use journalism categories like `Sustainability`, `Environment`, `Health`, `Politics`.
7. **Menus:** `_data/menu.yml` only defines `main` (Home, Media, About, Contact). Footers are disabled. The `/blog/` index exists in `blog/index.html` but is currently omitted from main navigation.

---

## 7. Developer & Agent Playbooks (Common Workflows)

### Workflow A: Add a Written Article / Post Card
1. Create a new markdown file in `collections/_posts/YYYY-MM-DD-article-slug.md`.
2. Populate the front matter:
   ```yaml
   ---
   layout: post
   title: "Article Headline or Outlet Name"
   categories: ["Journalism", "Environment"]
   description: "1-2 sentence summary of the piece."
   thumbnail: "/assets/images/gen/blog/slug-thumbnail.jpg"
   image: "/assets/images/gen/blog/slug-thumbnail.jpg"
   custom_url: "https://external-publication.com/article-url"
   ---
   ```
3. Add thumbnail image to `assets/images/gen/blog/`.

### Workflow B: Add a Media Appearance (Radio, TV, Podcast)
1. Create a new markdown file in `collections/_media/YYYY-MM-DD-outlet-slug.md`.
2. Choose the appropriate player type and populate:
   ```yaml
   ---
   title: "Outlet Name (e.g. RTÉ 2FM or Newstalk Daily)"
   categories: ["Radio", "Interview"]
   description: "Summary of topic discussed."
   date: YYYY-MM-DD
   outlet: "Outlet Name"
   link: "https://original-link.com"
   thumbnail: "/assets/images/gen/media/slug.jpg"
   
   # For RTÉ:
   rte_clip_id: "12345678"
   rte_start_seconds: 120
   rte_start_label: "2:00"

   # OR for direct MP3:
   # audio_src: "https://audio-cdn.com/stream.mp3"
   # audio_start_seconds: 104
   # audio_start_label: "1:44"

   # OR for YouTube:
   # youtube_id: "xxxxxxxxxxx"
   # youtube_start_seconds: 30
   # youtube_start_label: "0:30"
   ---
   ```
3. Add thumbnail image to `assets/images/gen/media/`.

### Workflow C: Edit Homepage Text or Bio
* **Hero Headline & Description:** Edit `title` and `description` in [`pages/home.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/pages/home.md).
* **Extended Bio:** Edit markdown body in [`pages/about.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/pages/about.md).
* **Social Links:** Edit URLs in [`_data/social.json`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_data/social.json).
* **Navigation Items:** Edit list in [`_data/menu.yml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_data/menu.yml).

### Workflow D: Custom CSS Changes
* All custom styling must go into [`_sass/theme/_custom.scss`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_sass/theme/_custom.scss).
* If introducing new global color variables, define both light and dark values in [`_config.yml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_config.yml) and wire them in [`assets/css/main.scss`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/assets/css/main.scss).

---

## 8. Build, Environment & Deployment Notes

* **Ruby & Bundler Version:** Locked in [`Gemfile.lock`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/Gemfile.lock) to Bundler `2.3.11`.
* **Local Build Commands:**
  ```bash
  bundle install
  bundle exec jekyll serve   # Starts local dev server at http://localhost:4000
  bundle exec jekyll build   # Compiles source into _site/
  ```
* **Netlify Builds:** Automated on git push via [`netlify.toml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/netlify.toml) using Ruby 3.2.2 and command `jekyll build`.
* **`_site/` in Git:** While Netlify builds from source, `_site/` is currently tracked in git. When committing significant layout or content changes, ensure `_site` is regenerated or handled according to team conventions.

---
*Document compiled and verified against repository source code.*
