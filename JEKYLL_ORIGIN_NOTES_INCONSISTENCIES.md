# Jekyll Origin Notes Inconsistencies

This note compares `jekyll-origin-docs-structured-notes.md` against the current repository state.

The structured notes are useful as a vendor-docs reference for the Zerostatic Jekyll Origin theme, but this repository is a customized and partly stale theme instance. Some paths, defaults, and privacy assumptions in the notes do not match the actual files and configuration in this repo.

## 1. Include Paths Differ From This Repo

The structured notes reference docs-style include paths such as:

- `_includes/framework/global/cookies/cookie-consent.html`
- `_includes/framework/global/head/seo-meta-tags.html`
- `_includes/framework/global/head/og-meta-tags.html`

In this repository, those includes are flat under `_includes/framework/`:

- `_includes/framework/cookie-consent.html`
- `_includes/framework/seo-meta-tags.html`
- `_includes/framework/og-meta-tags.html`

The concepts in the notes are correct, but these paths are not correct for this checked-out theme tree.

## 2. Dark Mode Uses `localStorage`, Not `sessionStorage`

The notes say the manual dark-mode preference is saved in `sessionStorage`.

This repository uses `localStorage`:

- `_layouts/default.html` checks `localStorage.getItem('darkMode')`.
- `assets/js/darkModeSwitch.js` reads and writes `localStorage`.

Practical effect: the selected dark-mode preference can persist beyond a browser session, depending on browser storage behavior.

## 3. Logo Files Are SVGs, Not The PNG Defaults Listed In The Notes

The notes list default logo filenames such as:

- `assets/images/logo/logo.png`
- `assets/images/logo/logo-mobile.png`
- `assets/images/logo/logo-invert.png`
- `assets/images/logo/logo-invert-mobile.png`

This repository uses SVG paths configured in `_config.yml`:

- `assets/images/logo/logo.svg`
- `assets/images/logo/logo-mobile.svg`
- `assets/images/logo/logo-invert.svg`
- `assets/images/logo/logo-invert-mobile.svg`

This is not a functional issue because the notes also mention that alternate filenames and SVGs are supported. It is still a mismatch with the listed default asset names.

## 4. Post Collection Path Ambiguity Is Resolved Here

The notes mention that the vendor docs use both `_collections/posts` and `collections/_posts`.

This repository is configured with:

```yaml
collections_dir: collections
```

Current post files live in:

- `collections/_posts/`

For this repo, new posts should be added to `collections/_posts/`, not `_collections/posts`.

## 5. Privacy Defaults In The Notes Do Not Match Current Config

The notes describe the theme as privacy-aware and note that the theme does not use cookies by default.

The current repository still has several optional or demo integrations configured:

- `cookie_banner.enabled: true`
- Mailchimp form action points to a Zerostatic list.
- Disqus shortname is set to `zerostatic`.
- Some posts set `comments: true`.
- Formspree endpoint is configured.
- Privacy page mentions Plausible, Mailchimp, and Disqus even though analytics IDs are blank and the page copy appears generic.

This does not necessarily break the site, but the current config is not a clean privacy-default setup.

## 6. YouTube Embed Privacy Behavior Is Mixed

The notes summarize the theme/changelog as using privacy-enhanced YouTube embeds.

This repository contains both privacy-enhanced and normal YouTube embed URLs:

- `_includes/framework/shortcodes/youtube.html` uses `https://www.youtube-nocookie.com/embed/...`.
- `_includes/theme/cards/card-post.html` uses `https://www.youtube.com/embed/...`.
- `_layouts/home.html` contains a commented iframe using `https://www.youtube.com/embed/...`.

Most current media entries use thumbnails and external links rather than `youtube_id`, but if the card component's `youtube_id` path is used, it will load from the normal YouTube embed domain.

## 7. Category Pages Are Stock Theme Categories, Not Current Post Categories

The notes describe category support as part of the theme model.

Current source category files are stock theme examples:

- `categories/branding.md`
- `categories/design.md`
- `categories/development.md`
- `categories/hosting.md`
- `categories/javascript.md`
- `categories/marketing.md`
- `categories/product.md`
- `categories/seo.md`
- `categories/web-design.md`

Current posts use journalism-specific categories instead, including:

- `Sustainability`
- `Transport`
- `Environment`
- `Politics`
- `Conservation`
- `Climate Change`
- `Journalism`
- `Health`
- `Lifestyle`
- `Podcast`
- `Circular Economy`

Category links from real posts may point to category pages that do not exist in source, while existing stock category pages may be empty or irrelevant.

## 8. Footer Menu Documentation Is Not Active In This Site

The notes correctly describe footer and bottom menu groups supported by the theme:

- `footer_primary`
- `footer_secondary`
- `footer_tertiary`
- `bottom`

This repo's `_data/menu.yml` currently only defines:

- `main`

The footer and bottom areas are disabled in `_config.yml`:

```yaml
footer:
  enable_footer: false

bottom:
  enable_bottom: false
```

So the footer documentation is valid for the theme, but inactive in the current website.

## 9. Site URL And Social Metadata Still Point To The Theme Vendor/Demo

The notes explain that SEO and Open Graph output are generated from `_config.yml` and page front matter.

Current config still contains vendor/demo values:

```yaml
url: "https://jekyll-origin.netlify.app"

open_graph:
  meta_twitter_site: "@zerostaticio"
  meta_twitter_creator: "@zerostaticio"
```

For a production Rosalind Skillen site, these should be replaced with the real site URL and appropriate social account values.

## 10. Contact Form Uses Formspree Instead Of The Default Documented Netlify Path

The notes say Netlify Forms are the default documented contact form option, with Formspree as an alternative.

This repository currently uses Formspree:

```yaml
contact_form:
  use_netlify_form: false
  use_formspree_form: true
  formspree_endpoint: https://formspree.io/f/xldqwdqb
```

This is a valid supported configuration, but it is a deliberate divergence from the default documented setup.

## Summary

`jekyll-origin-docs-structured-notes.md` is broadly accurate for the Jekyll Origin theme, but it should not be treated as an exact map of this repository.

The main practical mismatches are:

- Some include paths in the notes do not exist in this repo.
- Dark mode persistence uses `localStorage`, not `sessionStorage`.
- The actual logo assets are SVGs.
- Posts belong in `collections/_posts/`.
- The repo still carries demo/vendor config for URL, Open Graph, Mailchimp, Disqus, and legal/privacy copy.
- Current post categories do not match the checked-in category pages.
- Footer docs describe supported features that are currently disabled.
- Formspree is active instead of Netlify Forms.
