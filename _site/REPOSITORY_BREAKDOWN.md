# Repository Breakdown

This repository is a Jekyll static site for Rosalind Skillen's personal journalism portfolio. It is built from the Zerostatic "Jekyll Origin" theme, with custom content, navigation, colors, fonts, images, posts, and media entries layered on top.

The generated site is committed in `_site/`, but the editable source of truth is the Jekyll source outside `_site/`.

## High-Level Summary

- **Site type:** Static Jekyll portfolio/blog site.
- **Primary purpose:** Personal homepage for an award-nominated journalist, with portfolio links and media appearances.
- **Theme base:** Jekyll Origin Premium Theme by Zerostatic.
- **Build output:** `_site/`.
- **Deploy target:** Netlify, configured by `netlify.toml`.
- **Main content areas:** homepage, media page, about page, contact page, portfolio/blog page, category pages, privacy/terms pages.
- **Main source directories:** `pages/`, `collections/`, `_layouts/`, `_includes/`, `_data/`, `_sass/`, `assets/`.
- **Main dependencies:** Jekyll 4.3, `jekyll-paginate`, `jekyll-environment-variables`, `webrick`.

## Repository Root

| Path | Role |
| --- | --- |
| `_config.yml` | Main Jekyll and theme configuration. Controls URL settings, collections, pagination, colors, fonts, logo, analytics, dark mode, comments, newsletter, contact form, footer, and bottom bar. |
| `Gemfile` | Ruby dependency declaration for Jekyll and plugins. |
| `Gemfile.lock` | Locked dependency versions. Requires Bundler `2.3.11`. |
| `netlify.toml` | Netlify build configuration. Runs `jekyll build`, publishes `_site`, sets `JEKYLL_ENV=production`, and requests Ruby `3.2.2`. |
| `README.md` | Original theme documentation for Jekyll Origin. Mostly generic, not project-specific. |
| `LICENSE` | Theme license. |
| `CODE_STRUCTURE.md` | Existing high-level editing guide. |
| `project_overview.md` | Existing short overview of the Jekyll structure. |
| `image_analysis.md` | Existing image usage notes. Some details appear stale relative to the current repository. |
| `todo.md` | Existing short task list. |
| `_site/` | Generated static output. Do not edit directly; rebuilds overwrite it. |

## Build And Runtime Model

Jekyll reads `_config.yml`, pages, collections, layouts, includes, Sass, and assets, then generates static HTML/CSS/JS in `_site/`.

Typical commands from the root:

```sh
bundle install
bundle exec jekyll serve
bundle exec jekyll build
```

Netlify uses:

```toml
[build]
  command = "jekyll build"
  publish = "_site"
```

The local build was not verified in this environment because the system Ruby installation could not find Bundler `2.3.11`, which is the Bundler version pinned in `Gemfile.lock`.

Observed error:

```text
Could not find 'bundler' (2.3.11) required by Gemfile.lock
```

To build locally, install the matching Bundler version or use the Ruby/Bundler version expected by the project:

```sh
gem install bundler:2.3.11
bundle install
bundle exec jekyll build
```

## Jekyll Configuration

The core Jekyll settings live in `_config.yml`.

Important settings:

- `baseurl: ""`
- `url: "https://jekyll-origin.netlify.app"`
- `permalink: pretty`
- `markdown: kramdown`
- `highlighter: rouge`
- `paginate: 6`
- `paginate_path: "/blog/page/:num"`
- `collections_dir: collections`

The configured plugins are:

- `jekyll-environment-variables`
- `jekyll-paginate`

The configured collections are:

| Collection | Directory | Output | Permalink |
| --- | --- | --- | --- |
| `posts` | `collections/_posts/` | `true` | `/blog/:path/` |
| `media` | `collections/_media/` | `true` | `/media/:path/` |

Defaults:

- Posts default to `layout: post` and `weight: 999`.
- Files in `categories/` get permalink `/category/:basename/`.
- Files in `pages/` get permalink `/:basename/`, unless the page front matter overrides it.

## Site-Wide Theme Settings

The project customizes the Origin theme through `_config.yml`.

### Branding

- Site title: `Rosalind Skillen`
- Logo text: `Rosalind Skillen`
- Logo images:
  - `assets/images/logo/logo.svg`
  - `assets/images/logo/logo-mobile.svg`
  - `assets/images/logo/logo-invert.svg`
  - `assets/images/logo/logo-invert-mobile.svg`

### Colors

The primary accent color is `#EC255A`. Light and dark mode colors are defined in `_config.yml` and converted into CSS custom properties in `assets/css/main.scss`.

### Fonts

The config disables Google-hosted fonts and enables self-hosted fonts:

- Heading: `Schibsted Grotesk`
- Body: `Open Sans`
- Monospace: `Fira Mono`
- Logo: `Schibsted Grotesk`

Font files are under `assets/fonts/`, and font-face declarations are in `assets/css/fonts.css`.

### Header

- Header is not fixed: `header.fixed: false`.
- Main menu appears on desktop.
- Hamburger/mobile menu is included.
- Dark mode toggle appears in the header.

### Footer And Bottom Bar

Both are disabled:

- `footer.enable_footer: false`
- `bottom.enable_bottom: false`

Footer and bottom templates still exist and can be re-enabled by config.

## Data Files

The `_data/` directory stores structured data used by templates.

| File | Purpose |
| --- | --- |
| `_data/menu.yml` | Main navigation links. Currently Home, Media, About, Contact. |
| `_data/social.json` | Social icons and links for X, GitHub, Instagram, LinkedIn, RSS. |
| `_data/contact.yml` | Contact details. Currently email only. |
| `_data/authors.yml` | Theme author records. Mostly stock/sample people, not obviously used by current posts. |
| `_data/partners.json` | Theme partner/logo data. Appears stock and not active in current layouts. |

Navigation is generated from `_data/menu.yml`. Social links are generated from `_data/social.json` by the social include.

## Content Structure

### Static Pages

Static pages live in `pages/`.

| File | URL | Layout | Notes |
| --- | --- | --- | --- |
| `pages/home.md` | `/` | `home` | Main homepage. Includes title, description, hero image, social links, and portfolio cards. |
| `pages/about.md` | `/about/` | `basic` | About page with image and biographical copy. |
| `pages/media.md` | `/media/` | `media-3` | Media appearances listing sourced from `site.media`. |
| `pages/contact.md` | `/contact/` | `contact` | Contact form page. |
| `pages/success.md` | `/contact/success` | `basic` | Contact form success page. |
| `pages/categories.md` | `/blog/categories/` | `categories` | Category index. |
| `pages/privacy.md` | `/privacy-policy/` | `basic` | Privacy policy. Contains some generic/theme copy. |
| `pages/terms.md` | `/terms-and-conditions/` | `basic` | Terms page. Contains generic placeholder legal copy. |

### Blog / Portfolio Pages

The blog/portfolio entry points are in `blog/`.

| File | URL | Layout | Notes |
| --- | --- | --- | --- |
| `blog/index.html` | `/blog/` | `blog` | Main portfolio listing. Uses three-column cards. |
| `blog/blog-2.html` | `/blog-2/` | `blog-2` | Alternate stock layout. Not linked from main menu. |
| `blog/blog-3.html` | `/blog-3/` | `blog-3` | Alternate stock layout. Not linked from main menu. |

`jekyll-paginate` only paginates `blog/index.html`, so the alternate blog pages are static layout examples unless manually swapped into the index.

### Categories

Category pages live in `categories/`.

Current category files:

- `branding.md`
- `design.md`
- `development.md`
- `hosting.md`
- `javascript.md`
- `marketing.md`
- `product.md`
- `seo.md`
- `web-design.md`

These are stock theme categories and do not match most current post categories, which include values such as `Sustainability`, `Transport`, `Environment`, `Politics`, `Conservation`, `Climate Change`, `Journalism`, `Health`, `Lifestyle`, `Podcast`, and `Circular Economy`.

This means many current portfolio categories can render as links to category URLs that do not have a matching source file.

### Posts Collection

Portfolio/blog entries live in `collections/_posts/`.

Current post entries:

| File | Title | External URL? | Notes |
| --- | --- | --- | --- |
| `2017-12-12-eolas.md` | eolas | Yes | Dublin City Transport Plan article. Has `post: eolas magazine` but no explicit `layout`; default applies. |
| `2020-09-10-irish_news.md` | Irish News | Yes | Column link. |
| `2021-02-27-currency.md` | The Currency | Yes | Article link. |
| `2021-07-20-bbc.md` | BBC Young Climate Reporter | Yes | BBC NI COP26 work. |
| `2021-07-23-sunday-times.md` | The Sunday Times | Yes | External Times link. |
| `2021-07-26-the-detail.md` | The Detail | Yes | Reporter page link. |
| `2022-02-10-bel-tel.md` | Belfast Telegraph | Yes | Tag page link. |
| `2023-01-11-irish-farmers-journal.md` | Irish Farmers Journal | Yes | Has `comments: true`, which can trigger Disqus if configured. |
| `2024-01-01-podcast.md` | Sustainable Conversations Podcast | Yes | Has `comments: true`, external Spotify link. |

The homepage sorts posts by date by default and shows up to 9 cards. The card URL uses `custom_url` when present, so these homepage/blog cards generally send users to external publications rather than local post pages.

### Media Collection

Media entries live in `collections/_media/` and render through the media page layouts.

Current media entries:

| Date | Title | Outlet | Link Type |
| --- | --- | --- | --- |
| 2021-11-03 | BBC Radio Ulster | BBC Radio Ulster | BBC programme link |
| 2021-11-04 | Tedx Talks | Tedx Talks | YouTube |
| 2022-10-26 | Keynote at Green Foundation Ireland conference | Green Foundation Ireland | YouTube |
| 2023-05-31 | Green Foundation Ireland | Green Foundation Ireland | YouTube |
| 2023-11-14 | Bridging The Atlantic | Bridging The Atlantic | YouTube |
| 2024-10-21 | Dublin Freelance Forum | Dublin Freelance Forum | External podcast page |
| 2025-12-08 | Ireland AM | Virgin Media | YouTube |
| 2026-01-10 | RTE 2FM | RTE 2FM | RTE radio link |
| 2026-01-10 | Tipp FM | Tipp FM | Tipp FM link |
| 2026-01-18 | Beat 102 103 | Beat 102 103 | External podcast link |
| 2026-02-11 | RTE 2FM | RTE 2FM | RTE radio link |

The active `pages/media.md` uses `layout: media-3`, which sorts `site.media` by date descending and renders two-column full cards.

## Layouts

Layouts live in `_layouts/`.

| Layout | Purpose |
| --- | --- |
| `default.html` | Base HTML document. Includes SEO tags, Open Graph tags, feed, favicon, CSS, dark mode setup, cookie consent, fonts, GTM, mobile menu, header, wrapper, optional footer/bottom, scripts, and analytics includes. |
| `home.html` | Homepage layout. Renders the title/hero block, then post cards from `site.posts`, then an optional newsletter signup. |
| `basic.html` | Generic content page layout with title, description, image, and Markdown content. Used by About, Privacy, Terms, Success. |
| `contact.html` | Contact page layout. Shows title, contact form, optional contact sidebar, and optional page content. |
| `blog.html` | Main three-column post listing layout. |
| `blog-2.html` | Alternate row-style post listing. |
| `blog-3.html` | Alternate large/two-column post listing. |
| `post.html` | Default single-post layout. |
| `post-2.html` | Alternate single-post layout with image first. |
| `post-3.html` | Alternate split image/text single-post layout. |
| `media.html` | Row-style media listing. Uses `outlet` as card description. |
| `media-3.html` | Active media listing. Two-column/full card style. |
| `categories.html` | Category index page. |
| `category.html` | Single category page that lists posts in `site.categories[page.title]`. |

## Include Components

Reusable Liquid/HTML components live in `_includes/`.

The repository contains a large stock set under `_includes/framework/`, including:

- header, logo, menus, hamburger, mobile menu
- title/hero/heading components
- post/project cards
- category links and category badges
- author display
- contact display and contact forms
- footer and bottom bar
- social links
- newsletter subscribe form
- cookie consent
- comments integrations
- analytics integrations
- paginator components
- favicon, feed, SEO, Open Graph tags

There is one active custom/theme override under:

- `_includes/theme/cards/card-post.html`

This card component supports:

- title
- description
- thumbnail
- YouTube embed ID
- URL
- authors
- categories
- client
- card style variants: `column`, `row`, `list`, `full`
- optional read-more link

The active homepage, blog, category, and media listing layouts all use this card component.

## Styling System

The CSS entry point is:

- `assets/css/main.scss`

This file has Jekyll front matter, defines CSS custom properties from `_config.yml`, defines dark-mode custom properties, then imports `_sass/style.scss`.

Import chain:

```scss
assets/css/main.scss
  -> _sass/style.scss
    -> _sass/framework/index.scss
    -> _sass/theme/index.scss
      -> _sass/theme/_custom.scss
```

Important style areas:

- `_sass/framework/bootstrap/` contains Bootstrap source partials.
- `_sass/framework/libraries/font-awesome/` contains Font Awesome Sass.
- `_sass/framework/zerostatic/` contains the stock Origin theme components.
- `_sass/theme/_custom.scss` contains project/theme overrides.
- `_sass/theme/_variables.scss` exists but is not imported by `theme/index.scss` in the current tree.

Notable custom style changes in `_sass/theme/_custom.scss`:

- Adjusts logo image spacing.
- Sets title heading weight and underline treatment.
- Customizes title description typography.
- Overrides desktop main menu behavior.
- Styles the "More Posts" button.
- Customizes post headers for the three post layouts.
- Customizes `.card-post` and its row/list/full variants.
- Adds responsive `.video-container` styles for embeds.

## Assets

Assets live under `assets/`.

### CSS

- `assets/css/main.scss`: compiled by Jekyll into `main.css`.
- `assets/css/fonts.css`: self-hosted font declarations.
- `assets/css/cookieconsent.css`: cookie consent styling.

### JavaScript

| File | Purpose |
| --- | --- |
| `assets/js/scripts.js` | Theme script entry point. |
| `assets/js/hamburger.js` | Mobile menu toggle behavior. |
| `assets/js/darkModeSwitch.js` | Dark mode switching. |
| `assets/js/header.js` | Fixed header behavior, only loaded when fixed header is enabled. |
| `assets/js/comments.js` | Comment-related script. |
| `assets/js/cookieconsent.js` | Cookie banner behavior. |

### Fonts

Self-hosted fonts are included for:

- Fira Mono
- Open Sans
- Schibsted Grotesk
- Font Awesome

### Images

Important active images include:

- `assets/images/rosalind-skillen-homepage-hero.png`
- `assets/images/gen/content/rosalind-skillen-about-me.jpeg`
- `assets/images/gen/blog/*` portfolio thumbnails
- `assets/images/gen/media/*` media thumbnails
- `assets/images/logo/*` logo variants
- `assets/images/favicon/favicon.png`
- `assets/images/og/og-twitter-blog-image.webp`

The repository also includes many stock/generated theme images such as `blog-1-*`, `content-1-*`, menu icons, team headshots, and partner-related references. Some of these appear unused by the current site.

## Render Flow

### Homepage

1. `pages/home.md` defines front matter for `/`.
2. It uses `_layouts/home.html`.
3. `home.html` includes `framework/title.html` for the headline, description, image, and social links.
4. It pulls cards from `site.posts`.
5. It sorts by `date` unless changed in front matter.
6. It limits to 9 cards.
7. It renders each card with `_includes/theme/cards/card-post.html`.
8. If `custom_url` exists on a post, cards link to that external URL.
9. If Mailchimp config is present, the newsletter block is rendered.

### Blog / Portfolio

1. `blog/index.html` uses `_layouts/blog.html`.
2. The layout reads `site.posts`, or `paginator.posts` when pagination is active.
3. Posts are rendered as three-column cards.
4. Pagination controls are rendered when `paginator` exists.

### Media

1. `pages/media.md` uses `_layouts/media-3.html`.
2. The layout reads `site.media`.
3. It sorts by `date` descending.
4. It renders two-column full-style cards.
5. Cards link to each media entry's external `link`.

### Contact

1. `pages/contact.md` uses `_layouts/contact.html`.
2. `_config.yml` sets `use_formspree_form: true`.
3. The layout includes `_includes/framework/form-contact-formspree.html`.
4. The form posts to `https://formspree.io/f/xldqwdqb`.
5. Contact sidebar uses `_data/contact.yml` when present.

## Generated Output

`_site/` is checked into the repository and contains generated HTML/CSS/JS/assets. It includes pages such as:

- `_site/index.html`
- `_site/about/index.html`
- `_site/contact/index.html`
- `_site/blog/index.html`
- `_site/media` output depending on build state
- `_site/category/*`
- `_site/privacy-policy/index.html`
- `_site/terms-and-conditions/index.html`

Because `_site/` is generated, source edits should be made outside `_site/` and then rebuilt.

## Current Customization State

The repository is partly customized and partly still stock theme.

Customized/project-specific:

- Home page copy and hero image.
- About page copy and image.
- Media appearances collection.
- Portfolio/blog collection with external article links.
- Primary menu.
- Social links.
- Contact email and Formspree endpoint.
- Accent color and typography choices.
- Logo files.
- Card styling overrides.

Still stock or likely stale:

- `README.md` is theme documentation, not project documentation.
- `_data/authors.yml` contains sample team members.
- `_data/partners.json` points all partner URLs to Zerostatic.
- `categories/` contains stock categories that do not match current post categories.
- `pages/terms.md` contains placeholder references to Example Site, California, and example email.
- `pages/privacy.md` contains `support@example.com` and mentions Plausible/Mailchimp/Disqus even though analytics IDs are blank and actual usage should be confirmed.
- `_config.yml` still uses `url: "https://jekyll-origin.netlify.app"`.
- Open Graph Twitter fields still point to `@zerostaticio`.
- Disqus shortname is `zerostatic`; comments are enabled on two posts.
- Mailchimp signup URL points to a Zerostatic account.
- Several stock images, icons, team photos, and generated theme images appear unused.
- `.DS_Store` files are present in the repository tree.

## Potential Issues And Cleanup Targets

### 1. Production URL Still Points To Theme Demo

`_config.yml` sets:

```yaml
url: "https://jekyll-origin.netlify.app"
```

This should be changed to the real production domain. It affects canonical URLs, feeds, SEO metadata, and social previews.

### 2. Social Metadata Still Uses Zerostatic

Open Graph/Twitter config has:

```yaml
meta_twitter_site: "@zerostaticio"
meta_twitter_creator: "@zerostaticio"
```

These should likely be replaced with Rosalind's handle or removed if not needed.

### 3. Category Source Files Do Not Match Current Posts

The repository has stock category files like `Branding`, `Design`, and `Javascript`, but posts use journalism-specific categories. Category links for current posts may resolve to missing or empty pages unless matching category files are added.

Recommended action: replace stock category files with source files for the current categories.

### 4. Newsletter And Comments May Use Theme Owner Accounts

Mailchimp points to a Zerostatic list. Disqus shortname is `zerostatic`.

If newsletter/comments are not intentionally used, disable or remove them:

```yaml
newsletter:
  mailchimp:
    form_action_url: ""

comments:
  disqus:
    shortname: ""
```

### 5. Legal Pages Contain Placeholder Text

`pages/terms.md` references Example Site, California, example.com, and example@example.com.

`pages/privacy.md` references support@example.com and mentions services that may not actually be active.

These should be reviewed before publishing.

### 6. Build Environment Needs Matching Bundler

Local build failed because Bundler `2.3.11` is missing. Netlify asks for Ruby `3.2.2`, which may work if it installs the locked Bundler or uses a compatible environment.

Recommended local setup:

```sh
gem install bundler:2.3.11
bundle install
bundle exec jekyll build
```

### 7. `_site/` Is Committed

The generated output is committed. This is workable for GitHub Pages-style publishing, but Netlify can build from source and does not require `_site/` to be committed.

If Netlify is the deployment source of truth, consider ignoring `_site/` to reduce churn.

### 8. Unused Theme Assets

The repository includes stock theme images, team portraits, many generated `blog-*` and `content-*` images, and menu icon sets. These increase repository size and make asset maintenance harder.

Recommended action: remove assets only after confirming they are not referenced by Liquid templates, data files, Markdown front matter, CSS, or generated output needs.

### 9. `.DS_Store` Files

`.DS_Store` files are present under the repository and assets directories. They should generally be removed and ignored.

### 10. Existing Documentation Is Fragmented

There are already multiple markdown files:

- `README.md`
- `project_overview.md`
- `CODE_STRUCTURE.md`
- `image_analysis.md`
- `todo.md`

This new file is more complete, but the repo may benefit from consolidating docs into a project-specific README and moving old notes into an archive or removing stale docs.

## Recommended Editing Guide

### Change Homepage Text

Edit:

- `pages/home.md`

Relevant fields:

- `title`
- `description`
- `image`
- `meta_title`
- `meta_description`
- `posts`

### Add A Portfolio/Article Card

Add a file to:

- `collections/_posts/YYYY-MM-DD-slug.md`

Recommended front matter:

```yaml
---
layout: post
title: "Publication Name"
categories: ["Journalism", "Health"]
description: "Short summary for cards."
thumbnail: "/assets/images/gen/blog/example-thumbnail.jpg"
image: "/assets/images/gen/blog/example-thumbnail.jpg"
custom_url: "https://external-publication-url.example/article"
---
```

If `custom_url` is set, homepage and listing cards can link to the external article.

### Add A Media Appearance

Add a file to:

- `collections/_media/YYYY-MM-DD-slug.md`

Recommended front matter:

```yaml
---
title: "Outlet or Event"
description: "Short description of the appearance."
date: YYYY-MM-DD
outlet: "Outlet Name"
link: "https://external-url.example"
thumbnail: "/assets/images/gen/media/example.jpg"
---
```

### Change Navigation

Edit:

- `_data/menu.yml`

Current menu entries are Home, Media, About, Contact.

### Change Social Links

Edit:

- `_data/social.json`

The icons use Font Awesome classes such as `fab fa-github` and `fab fa-linkedin`.

### Change Contact Info

Edit:

- `_data/contact.yml`
- `_config.yml` under `contact_form`

The current contact form uses Formspree.

### Change Theme Colors Or Fonts

Edit:

- `_config.yml` under `colors`
- `_config.yml` under `fonts`
- `assets/css/fonts.css` if changing self-hosted fonts

### Make CSS Changes

Prefer editing:

- `_sass/theme/_custom.scss`

Avoid editing generated CSS in `_site/`.

### Change Layout Structure

Edit layouts in:

- `_layouts/`

Or reusable components in:

- `_includes/framework/`
- `_includes/theme/`

For card changes, start with:

- `_includes/theme/cards/card-post.html`

## Suggested Next Steps

1. Replace `_config.yml` production URL and social metadata with real site values.
2. Decide whether newsletter and comments should be enabled; remove Zerostatic defaults if not.
3. Replace stock category files with categories that match the journalism portfolio.
4. Review and rewrite privacy/terms pages before launch.
5. Remove `.DS_Store` files and add them to `.gitignore`.
6. Audit and remove unused stock assets after reference checking.
7. Convert `README.md` from theme documentation into project-specific setup, editing, and deployment instructions.
8. Set up local Ruby/Bundler so `bundle exec jekyll build` works consistently.
