# Jekyll Origin Theme — Official Documentation & Supersession Guide

> **Official Upstream Source:** [https://www.zerostatic.io/docs/jekyll-origin/](https://www.zerostatic.io/docs/jekyll-origin/)  
> **Vendor:** Zerostatic Themes (Rob Austin)  
> **Theme Version:** 2.1.0 (Nov 24, 2023)  
> **Local Copy Status:** Complete with repository-specific supersession and divergence analysis.

---

## Overview

This directory houses the comprehensive local copy of the official Zerostatic **Jekyll Origin** theme documentation. Because this repository has introduced custom architectures, flattened structures, and journalism-specific features, this documentation explicitly marks:
1. **Vanilla Specification:** The official upstream design, configurations, and assumptions.
2. **Repository Implementation:** What is actually deployed and configured in this codebase.
3. **Supersession Status:** Clear guidance on what upstream features are active, modified, replaced, or disabled.

---

## Documentation Sections

| Section | Upstream URL | Local File | Summary & Repo Status |
| :--- | :--- | :--- | :--- |
| **01. Install & Setup** | [`/install/`](https://www.zerostatic.io/docs/jekyll-origin/install/) | [`01-install-getting-started.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/docs/jekyll-origin/01-install-getting-started.md) | Ruby, Bundler, local serve, Netlify configuration. |
| **02. Colors & Fonts** | [`/config/theme-color-font/`](https://www.zerostatic.io/docs/jekyll-origin/config/theme-color-font/) | [`02-config-theming-fonts.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/docs/jekyll-origin/02-config-theming-fonts.md) | Color variables, dark mode palette, self-hosted fonts. |
| **03. Logo, Header & Dark Mode** | [`/config/logo/`](https://www.zerostatic.io/docs/jekyll-origin/config/logo/), [`/header/`](https://www.zerostatic.io/docs/jekyll-origin/config/header/), [`/darkmode/`](https://www.zerostatic.io/docs/jekyll-origin/config/darkmode/) | [`03-config-logo-header-darkmode.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/docs/jekyll-origin/03-config-logo-header-darkmode.md) | SVG branding, header sticky settings, `localStorage` dark mode. |
| **04. Forms, Integrations & Cookies** | [`/config/contact-form/`](https://www.zerostatic.io/docs/jekyll-origin/config/contact-form/), [`/cookie-banner/`](https://www.zerostatic.io/docs/jekyll-origin/config/cookie-banner/), [`/comments/`](https://www.zerostatic.io/docs/jekyll-origin/config/comments/), [`/newsletter/`](https://www.zerostatic.io/docs/jekyll-origin/config/newsletter/), [`/analytics/`](https://www.zerostatic.io/docs/jekyll-origin/config/analytics/) | [`04-config-forms-integrations-cookies.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/docs/jekyll-origin/04-config-forms-integrations-cookies.md) | Formspree override, cookie banner, analytics, and comments. |
| **05. Menus, Footer & SEO** | [`/config/main-menu/`](https://www.zerostatic.io/docs/jekyll-origin/config/main-menu/), [`/footer/`](https://www.zerostatic.io/docs/jekyll-origin/config/footer/), [`/seo-meta-tags/`](https://www.zerostatic.io/docs/jekyll-origin/config/seo-meta-tags/), [`/og-meta-tags/`](https://www.zerostatic.io/docs/jekyll-origin/config/og-meta-tags/) | [`05-config-menus-seo-meta.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/docs/jekyll-origin/05-config-menus-seo-meta.md) | Navigation arrays, disabled footers, Open Graph tags. |
| **06. Pages & Layouts** | [`/pages/home/`](https://www.zerostatic.io/docs/jekyll-origin/pages/home/), [`/blog/`](https://www.zerostatic.io/docs/jekyll-origin/pages/blog/), [`/post/`](https://www.zerostatic.io/docs/jekyll-origin/pages/post/), [`/about/`](https://www.zerostatic.io/docs/jekyll-origin/pages/about/), [`/contact/`](https://www.zerostatic.io/docs/jekyll-origin/pages/contact/) | [`06-pages-posts-layouts.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/docs/jekyll-origin/06-pages-posts-layouts.md) | Templates for home, blog, post, about, and contact. |
| **07. Repo Extensions & Divergences** | *N/A (Project Specific)* | [`07-repo-extensions-and-divergences.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/docs/jekyll-origin/07-repo-extensions-and-divergences.md) | **Media collection (`_media/`), lazy RTÉ/audio/YouTube players, `custom_url`.** |

---

## Supersession & Divergence Matrix (Vanilla vs. This Repo)

| Architectural Area | Vanilla Zerostatic Origin Theme | Rosalind Skillen Repository | Supersession Impact |
| :--- | :--- | :--- | :--- |
| **Include File Hierarchy** | Deeply nested paths (e.g. `_includes/framework/global/cookies/cookie-consent.html`). | Flattened directly under `_includes/framework/` (e.g. `_includes/framework/cookie-consent.html`). | **SUPERSEDED.** Theme include calls reference flat filenames. |
| **Post / Card Links** | Posts are internal blog articles that link to local `/blog/:path/` pages. | Posts represent external journalism articles using `custom_url: "https://..."`. | **EXPANDED.** Cards link directly to publications (*BBC, Irish Farmers Journal, etc.*). |
| **Broadcast / Audio Media** | Not supported. Theme only features standard text posts and project cards. | Dedicated `collections/_media/` collection with layout [`media-3.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/media-3.html). | **NEW FEATURE.** Manages radio, podcast, TV, and conference appearances. |
| **Media Player Architecture** | Basic YouTube shortcode or static thumbnail link. | Interactive lazy players in [`_includes/theme/cards/card-post.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_includes/theme/cards/card-post.html) for RTÉ Bosco iframe, HTML5 `<audio>`, and YouTube with timestamp seeking. | **SUPERSEDED.** Drastically reduces initial load weight; loads players on demand. |
| **Dark Mode Storage** | Preserves user choice in `sessionStorage` (resets on window close). | Preserves user choice in `localStorage` in both script and layout head inline script. | **SUPERSEDED.** Theme choice persists permanently across sessions. |
| **Logo Formats** | Documented default assets are PNGs (`logo.png`, `logo-mobile.png`). | Configured with SVGs (`logo.svg`, `logo-invert.svg`) in [`_config.yml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_config.yml). | **CUSTOMIZED.** Scalable vector graphics replace raster images. |
| **Contact Form Provider** | Netlify Forms is default documented option. | Formspree is actively configured (`use_formspree_form: true`, endpoint active). | **SUPERSEDED.** Form submissions go to Formspree. |
| **Categories** | Stock design/tech categories (`branding`, `development`, `javascript`). | Journalism categories (`Sustainability`, `Health`, `Environment`, `Politics`). | **IN PROGRESS.** Source category files in `categories/` remain stock and should be updated. |
| **Footer & Bottom Bar** | Enabled by default with multi-column menus and copyright. | Explicitly disabled (`enable_footer: false`, `enable_bottom: false`) in `_config.yml`. | **SUPERSEDED.** Cleaner, single-focus page design. |
| **Site Metadata** | Zerostatic demo URLs and `@zerostaticio` Twitter handles. | Retains demo values in `_config.yml` (`url: "https://jekyll-origin.netlify.app"`). | **STALE DEMO VALUE.** Needs final production domain update. |

---

## Related Notes in Repository

* [`AGENT_ONBOARDING.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/AGENT_ONBOARDING.md): Master onboarding guide for AI agents and human developers.
* [`JEKYLL_ORIGIN_NOTES_INCONSISTENCIES.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/JEKYLL_ORIGIN_NOTES_INCONSISTENCIES.md): Detailed line-by-line divergence audit.
* [`media-card-lazy-audio-player-recipe.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/media-card-lazy-audio-player-recipe.md): Recipe for the lazy audio/RTÉ player card implementation.
* [`REPOSITORY_BREAKDOWN.md`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/REPOSITORY_BREAKDOWN.md): Complete repository inventory.
