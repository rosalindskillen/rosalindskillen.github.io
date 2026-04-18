# Jekyll Origin Documentation - Structured Notes

Created: 2026-04-18
Scope: Zerostatic Jekyll Origin documentation, including the docs landing page, every sidebar tab under `https://www.zerostatic.io/docs/jekyll-origin/`, and the Product Page/Changelog links exposed from the docs header.

This is a paraphrased working note, not a verbatim copy of the vendor documentation. It is designed as a practical reference for configuring and maintaining a Jekyll Origin site.

## Source Map

Main docs:

- Getting Started: https://www.zerostatic.io/docs/jekyll-origin/
- Install: https://www.zerostatic.io/docs/jekyll-origin/install/

Config tabs:

- Theme Colors and Fonts: https://www.zerostatic.io/docs/jekyll-origin/config/theme-color-font/
- Logo: https://www.zerostatic.io/docs/jekyll-origin/config/logo/
- Header: https://www.zerostatic.io/docs/jekyll-origin/config/header/
- Dark Mode: https://www.zerostatic.io/docs/jekyll-origin/config/darkmode/
- Contact Form: https://www.zerostatic.io/docs/jekyll-origin/config/contact-form/
- Cookie Banner: https://www.zerostatic.io/docs/jekyll-origin/config/cookie-banner/
- Comments: https://www.zerostatic.io/docs/jekyll-origin/config/comments/
- Newsletter: https://www.zerostatic.io/docs/jekyll-origin/config/newsletter/
- Analytics: https://www.zerostatic.io/docs/jekyll-origin/config/analytics/
- Footer: https://www.zerostatic.io/docs/jekyll-origin/config/footer/
- SEO Meta Tags: https://www.zerostatic.io/docs/jekyll-origin/config/seo-meta-tags/
- OG Meta Tags: https://www.zerostatic.io/docs/jekyll-origin/config/og-meta-tags/
- Main Menu: https://www.zerostatic.io/docs/jekyll-origin/config/main-menu/

Page/content tabs:

- Home: https://www.zerostatic.io/docs/jekyll-origin/pages/home/
- Blog: https://www.zerostatic.io/docs/jekyll-origin/pages/blog/
- Post: https://www.zerostatic.io/docs/jekyll-origin/pages/post/
- About/basic page: https://www.zerostatic.io/docs/jekyll-origin/pages/about/
- Contact page: https://www.zerostatic.io/docs/jekyll-origin/pages/contact/

Docs-header links:

- Product Page: https://www.zerostatic.io/theme/jekyll-origin/
- Changelog: https://www.zerostatic.io/theme/jekyll-origin/#changelog

## High-Level Mental Model

Jekyll Origin is a premium Jekyll blog theme. Most site-wide behavior is controlled through `_config.yml`, while navigation lives in `_data/menu.yml`, contact details live in `_data/contact.yml`, and page-specific behavior is driven by front matter.

The theme is built around:

- Markdown-based content pages.
- Blog and post layouts with multiple layout variants.
- Configurable SEO and Open Graph metadata.
- Privacy-aware defaults, including self-hosted fonts, privacy-friendly analytics options, a cookie banner, and optional Commento comments.
- Netlify and GitHub Pages readiness.
- SCSS, Bootstrap grid/utilities, Font Awesome icons, and no jQuery.

## Getting Started and Install

Relevant files and commands:

- Install Ruby and RubyGems first.
- Extract the purchased theme zip locally.
- Run `bundle install`.
- Run `bundle exec jekyll build`.
- Develop locally with `bundle exec jekyll serve`.
- Local preview URL: `http://localhost:4000`.
- The docs landing page notes that the theme includes a `netlify.toml`, so Netlify builds are preconfigured for Jekyll.

Practical checklist:

- Confirm the local Ruby version works with the theme.
- Run `bundle install` after extracting or updating the theme.
- Use `bundle exec jekyll serve` during editing.
- If deploying to Netlify, keep the provided `netlify.toml` unless you have a deliberate reason to change the build settings.

## Product Page and Theme Capabilities

Product metadata from the product page:

- Jekyll version: `4.3.2+`.
- Theme version: `2.1.0`.
- Updated: `Nov 24, 2023`.
- License: Pro.
- Listed price: `$49 USD`.

Major feature areas:

- Content types: homepage, blog index, posts, basic pages, and contact page.
- Layout variety: blog and post content each have three layout options.
- Design/code: responsive, front matter driven, and intended to be editable without hardcoded content.
- Styling: SCSS compiled by Jekyll, Bootstrap grid/media/utilities, small CSS/JS footprint, Font Awesome icons.
- SEO: automatic title, description, and Open Graph output with page-level overrides.
- Privacy: no cookies by default, self-hosted font support, privacy-friendly analytics choices, and cookie-managed GA/GTM.
- Integrations: Netlify/Formspree contact forms, Disqus/Commento comments, Mailchimp newsletter forms, GA4/GTM/Plausible/Umami analytics.
- Feeds/deploy: RSS support, Netlify-ready, GitHub Pages-ready.

Changelog summary:

- `2.0` on `Nov 24, 2023`: moved to the Jekyll Zerostatic framework with more components, fixes, and config options.
- `1.6` on `Oct 22, 2023`: added Plausible, Umami, self-hosted fonts, cookie banner, Commento, safer Disqus loading, YouTube no-cookie embeds, Mailchimp cleanup, RSS improvements, SEO/OG improvements, GTM support, Bootstrap 5.3.2, Jekyll 4.3, Netlify/GitHub Pages retesting, and removal of `jekyll-feed`.
- `1.2` on `Oct 5, 2022`: added RSS functionality and RSS footer icon support.
- `1.1` on `Jul 11, 2022`: updated author component, category generation, category pages, and minor fixes.
- `1.0` on `Jun 21, 2022`: initial release.

## Global Config: Theme Colors and Fonts

Primary file:

- `_config.yml`

Color configuration:

- `colors.primary_bg`
- `colors.primary_bg_2`
- `colors.primary_text`
- `colors.base_bg`
- `colors.base_bg_2`
- `colors.base_bg_3`
- `colors.base_text`
- `colors.base_text_2`
- `colors.logo_text`
- `colors.header_text`
- Dark-mode equivalents use `_dark` suffixes, for example `base_bg_dark`, `base_text_dark`, and `header_text_dark`.

Font configuration:

- `fonts.use_google_fonts`: enable external Google Fonts URL loading.
- `fonts.google_fonts_url`: stores the generated Google Fonts stylesheet URL.
- `fonts.use_self_hosted_fonts`: enables local font files.
- `fonts.heading`: heading font family.
- `fonts.base`: body font family.
- `fonts.monospace`: monospace font family.
- `fonts.logo`: logo text font family.

External Google Fonts workflow:

- Select fonts and weights in Google Fonts.
- Copy only the stylesheet URL into `fonts.google_fonts_url`.
- Set `use_google_fonts: true`.
- Set `use_self_hosted_fonts: false`.
- Update `heading`, `base`, `monospace`, and `logo` to match the selected font families.

Self-hosted fonts workflow:

- Put font files under `assets/fonts/`.
- Add matching `@font-face` definitions in `assets/css/fonts.css`.
- Ensure the `src` paths in `fonts.css` are relative to that CSS file.
- Set `use_google_fonts: false`.
- Set `use_self_hosted_fonts: true`.
- The docs point to `https://gwfh.mranftl.com/fonts` as one way to download Google font files and generate `@font-face` declarations.

Gotcha:

- Self-hosted font updates are more privacy-friendly, but require correct font files, CSS declarations, and relative paths.

## Global Config: Logo

Primary file:

- `_config.yml`

Primary asset folder:

- `assets/images/logo/`

Important logo config keys:

- `logo.logo_text`
- `logo.logo_image`
- `logo.logo_image_mobile`
- `logo.logo_image_invert`
- `logo.logo_image_invert_mobile`
- `logo.logo_image_desktop_height`
- `logo.logo_image_desktop_width`
- `logo.logo_image_mobile_height`
- `logo.logo_image_mobile_width`
- `logo.show_logo_image_on_mobile`
- `logo.show_logo_image_on_desktop`
- `logo.show_logo_text_on_mobile`
- `logo.show_logo_text_on_desktop`

Supported logo modes:

- Image-only logo.
- Text-only logo.
- Combined image and text logo.
- Different desktop/mobile combinations.
- Different normal/inverted image assets.

Expected default logo assets:

- `assets/images/logo/logo.png`: desktop image.
- `assets/images/logo/logo-mobile.png`: mobile image or compact mark.
- `assets/images/logo/logo-invert.png`: inverted desktop image, commonly for transparent headers.
- `assets/images/logo/logo-invert-mobile.png`: inverted mobile image.

Gotcha:

- You can replace the default files or point `_config.yml` to different filenames and extensions, including SVGs.

## Global Config: Header

Primary file:

- `_config.yml`

Important config:

- `header.fixed`

Behavior:

- The header can be static or fixed.
- Setting `header.fixed: true` makes it fixed.
- Fixed mode also enables the scroll animation behavior.

## Global Config: Dark Mode

Primary file:

- `_config.yml`

Important config keys:

- `darkmode.enable_dark_mode`
- `darkmode.show_dark_mode_toggle_in_header`
- `darkmode.show_dark_mode_toggle_in_bottom`
- `darkmode.show_dark_mode_toggle_in_mobile_menu`

Behavior:

- Dark mode can detect the user's operating-system preference through `prefers-color-scheme`.
- Manual light/dark selection overrides the system preference.
- The manual preference is saved in `sessionStorage`, so it persists across page loads during the session but can reset when the session/storage is cleared.
- Toggle placement is configurable for header, footer/bottom area, and mobile menu.

Related config:

- Dark-mode colors are set in the `colors.*_dark` keys in `_config.yml`.

## Global Config: Contact Form

Primary file:

- `_config.yml`

Important config keys:

- `contact_form.use_netlify_form`
- `contact_form.use_formspree_form`
- `contact_form.formspree_endpoint`
- `contact_form.netlify_form_name`

Supported providers:

- Netlify Forms.
- Formspree.

Netlify behavior:

- Netlify Forms are the default documented option.
- If the site is hosted on Netlify and Netlify Forms are enabled, the form is expected to work automatically with the configured form name.

Formspree behavior:

- Disable Netlify Forms.
- Enable Formspree.
- Add your Formspree endpoint.
- Clear `netlify_form_name` if you are not using Netlify.

## Global Config: Cookie Banner

Primary file:

- `_config.yml`

Advanced customization file:

- `_includes/framework/global/cookies/cookie-consent.html`

Important config keys:

- `cookie_banner.enabled`
- `cookie_banner.show_manage_cookies_at_bottom`

Behavior:

- The banner is based on the open-source CookieConsent project: https://github.com/orestbida/cookieconsent.
- The theme does not use cookies by default, so a banner may only be needed after adding cookie-setting integrations such as Google Analytics or Google Tag Manager.
- The banner does more than display a prompt: it can prevent tagged scripts from loading until the visitor consents.
- Rejected cookies prevent managed scripts from running and delete associated cookies.

Managed services:

- GA4.
- Google Tag Manager.

Implementation detail:

- Analytics scripts can be tagged with `data-cookiecategory="analytics"`.
- Visitors must accept all cookies or enable the analytics category for those scripts to run.
- For additional services, update the theme's cookie consent include and follow the CookieConsent project docs.

## Global Config: Comments

Primary file:

- `_config.yml`

Important config keys:

- `comments.commento.enabled`
- `comments.disqus.shortname`

Supported providers:

- Disqus.
- Commento.

Disqus:

- Add a Disqus shortname to enable it.
- Leave the shortname empty to disable it.
- The docs explicitly flag Disqus as less privacy-friendly because of extra cookies and tracking.

Commento:

- Enable Commento with `comments.commento.enabled: true`.
- Requires a Commento account.
- Must be used on the correct configured domain and is not expected to work locally.
- Presented by the docs as a privacy-friendlier paid option.

Per-post control:

- Set `comments: false` in a post's front matter to disable comments for that post.

## Global Config: Newsletter

Primary file:

- `_config.yml`

Important config keys:

- `newsletter.mailchimp.form_action_url`
- `newsletter.mailchimp.form_title`

Behavior:

- Newsletter subscription is powered by Mailchimp.
- Add your Mailchimp form action URL to enable subscribe boxes.
- Subscribe boxes can appear on the homepage and blog posts.

## Global Config: Analytics

Primary file:

- `_config.yml`

Important config keys:

- `analytics.google_analytics_id`
- `analytics.gtm_id`
- `analytics.plausible_data_domain`
- `analytics.umami_data_website_id`
- `analytics.umami_src`

Supported providers:

- Google Analytics / GA4.
- Google Tag Manager.
- Plausible.
- Umami.

Provider notes:

- Google Analytics is managed by the cookie banner if the banner is enabled.
- Google Tag Manager is also managed by the cookie banner if enabled.
- Plausible is highlighted by Zerostatic as a recommended privacy-friendly option.
- Umami requires both the website ID and script source URL.

Practical privacy pattern:

- Prefer Plausible or Umami where privacy matters.
- If using GA4 or GTM, keep the cookie banner enabled and verify the scripts are category-gated.

## Global Config: Footer

Primary files:

- `_config.yml`
- `_data/menu.yml`

Footer config keys:

- `footer.enable_footer`
- `footer.footer_title`
- `footer.footer_description`
- `footer.enable_social_media_icons`
- `footer.enable_menu_footer_primary`
- `footer.enable_menu_footer_secondary`
- `footer.enable_menu_footer_tertiary`
- `footer.footer_primary_menu_title`
- `footer.footer_secondary_menu_title`
- `footer.footer_tertiary_menu_title`

Bottom-bar config keys:

- `bottom.enable_bottom`
- `bottom.enable_bottom_menu`
- `bottom.copyright_text`
- `bottom.show_rss_icon`

Footer menu groups in `_data/menu.yml`:

- `footer_primary`
- `footer_secondary`
- `footer_tertiary`
- `bottom`

Behavior:

- The main footer can be shown or hidden.
- Social icons can be shown in the footer.
- Up to three footer menu columns are supported.
- A separate bottom area can be enabled for copyright, RSS, and bottom menu links.

## Global Config: SEO Meta Tags

Primary front matter keys:

- `title`
- `description`
- `image`
- `meta_title`
- `meta_description`
- `meta_image`
- `meta_keywords`

Theme include:

- `_includes/framework/global/head/seo-meta-tags.html`

Behavior:

- SEO tags are generated automatically for each page.
- `meta_title` overrides the normal title for SEO output.
- If `meta_title` is missing, the page `title` is used with the site title.
- If no page title exists, the site title is used.
- `meta_description` overrides `description`.
- `meta_keywords` is output only if present.
- `meta_image` is documented with the other metadata override fields, though image handling is more relevant to Open Graph output.

Practical pattern:

- Use `title`, `description`, and `image` for on-page content defaults.
- Use `meta_title`, `meta_description`, and `meta_image` when search/social metadata needs different wording or imagery.

## Global Config: OG Meta Tags

Primary file:

- `_config.yml`

Global Open Graph config keys:

- `open_graph.meta_opengraph_type`
- `open_graph.meta_twitter_card`
- `open_graph.meta_twitter_site`
- `open_graph.meta_twitter_creator`

Primary front matter keys:

- `title`
- `description`
- `image`
- `meta_title`
- `meta_description`
- `meta_image`

Theme include:

- `_includes/framework/global/head/og-meta-tags.html`

Behavior:

- Open Graph controls social preview output for platforms such as Twitter/X and Facebook.
- Global Open Graph/Twitter defaults come from `_config.yml`.
- Page-level OG output is generated automatically.
- `meta_title` overrides `title`.
- `meta_description` overrides `description`.
- `meta_image` overrides `image`.
- `og:url` uses the page URL converted to an absolute URL.

Practical pattern:

- Keep global Twitter/Open Graph account/card defaults in `_config.yml`.
- Set `image` for a normal page image.
- Set `meta_image` when the social sharing image should differ from the page image.

## Global Config: Main Menu

Primary files:

- `_config.yml`
- `_data/menu.yml`

Mobile menu config keys:

- `menu.show_dropdown_items_in_mobile_menu`
- `menu.show_social_media_in_mobile_menu`

Main menu group:

- `_data/menu.yml` -> `main`

Menu item fields:

- `name`
- `url`
- `weight`
- `child`
- Optional child item fields shown in docs include `icon`, `icon_darkmode`, and `description`.

Behavior:

- The main menu is edited in `_data/menu.yml`.
- Nested/dropdown menu items are added under `child`.
- The responsive/mobile menu uses the same `main` menu data.
- Mobile behavior can include or hide dropdown items and social links.

Practical pattern:

- Use `weight` to control ordering.
- Use `child` for dropdowns.
- Use icons/descriptions for richer dropdown items if your local theme markup supports them.

## Page Type: Home

Primary file:

- `pages/home.md`

Required layout:

- `layout: home`

Common front matter:

- `permalink: "/"`
- `title`
- `description`
- `image`
- `show_social_media_in_title`
- `meta_title`
- `meta_description`

Home posts block:

- `posts.heading`
- `posts.limit`
- `posts.sort`: `date` or `weight`
- `posts.view_more_button_text`
- `posts.view_more_button_link`
- `posts.columns`: `1`, `2`, `3`, or `4`
- `posts.show_authors`
- `posts.show_categories`

Behavior:

- The homepage can show a posts section.
- The default docs example enables posts and limits the count.
- Posts can be sorted by date or weight.

## Page Type: Blog

Primary file:

- `blog/index.html`

Supported layouts:

- `blog`
- `blog-2`
- `blog-3`

Behavior:

- The blog page uses front matter even though it is an HTML file.
- The docs explain that Jekyll pagination requires an HTML page for the blog index.
- The blog index automatically displays posts from the post collection.
- Change the blog layout by changing the `layout` front matter value.

## Page Type: Post

Primary content location:

- The docs describe posts as part of the posts collection.
- The text references `_collections/posts`.
- The example path references `collections/_posts/...`.
- Check the actual theme repo you are using before adding files, because the docs use both forms.

Supported layouts:

- `post`
- `post-2`
- `post-3`

Common front matter:

- `layout`
- `title`
- `date`
- `authors`
- `categories`
- `description`
- `thumbnail`
- `image`
- `comments`
- `meta_title`
- `meta_description`
- `meta_image`

Behavior:

- Posts are listed automatically on the blog page.
- Posts can also appear on the home page when the homepage posts section is enabled.
- Change a post layout by changing the `layout` front matter value.
- Set `comments: false` to disable comments for a specific post.

## Page Type: About / Basic Page

Primary example file:

- `pages/about.md`

Required layout:

- `layout: basic`

Common front matter:

- `title`
- `date`
- `permalink`
- `description`
- `image`

Behavior:

- The About page is mainly an example of the reusable `basic` layout.
- You can create additional basic pages by adding Markdown files in `pages/`.
- Use `permalink` to define the final URL for the page.

Practical example pattern:

- Add `pages/my-new-page.md`.
- Set `layout: basic`.
- Set a custom `permalink`, such as `/my-fancy-page/`.

## Page Type: Contact

Primary page layout:

- `layout: contact`

Primary data file:

- `_data/contact.yml`

Contact data fields:

- `phone`
- `fax`
- `email`
- `address`
- `map_link`
- `show_map_link`

Behavior:

- The contact page automatically displays the configured contact form.
- It also displays contact details from `_data/contact.yml`.
- The actual form provider is controlled by the `contact_form` block in `_config.yml`.

## Website Maintenance Checklist

When changing branding:

- Update color keys in `_config.yml`.
- Update font settings in `_config.yml`.
- If self-hosting fonts, add files under `assets/fonts/` and definitions in `assets/css/fonts.css`.
- Replace or repoint logo assets under `assets/images/logo/`.

When changing navigation:

- Edit `_data/menu.yml`.
- Use `main` for header/mobile navigation.
- Use `footer_primary`, `footer_secondary`, `footer_tertiary`, and `bottom` for footer areas.
- Adjust mobile menu options in `_config.yml`.

When adding a normal page:

- Add a Markdown file under `pages/`.
- Use `layout: basic` unless a specialized layout is needed.
- Set `permalink`, `title`, `description`, and `image`.
- Add SEO/OG overrides only when needed.

When adding a post:

- Add it to the theme's actual posts collection folder.
- Use one of the `post` layouts.
- Add authors, categories, thumbnail, image, and metadata.
- Decide whether comments should be enabled per post.

When configuring marketing or tracking:

- Add newsletter settings only after generating a Mailchimp form action URL.
- Prefer Plausible or Umami for privacy-friendly analytics.
- If using GA4 or GTM, enable and verify the cookie banner.
- Confirm that cookie-managed scripts are consent-gated.

When reviewing SEO/social previews:

- Start with `title`, `description`, and `image`.
- Add `meta_title`, `meta_description`, and `meta_image` for search/social overrides.
- Keep global Open Graph/Twitter defaults in `_config.yml`.
- Test social previews after publishing, especially if `meta_image` is changed.

## All Covered Tabs

- Covered docs landing page: Getting Started.
- Covered sidebar Install tab.
- Covered all 13 Config tabs: Theme Colors and Fonts, Logo, Header, Dark Mode, Contact Form, Cookie Banner, Comments, Newsletter, Analytics, Footer, SEO Meta Tags, OG Meta Tags, Main Menu.
- Covered all 5 Pages tabs: Home, Blog, Post, About/basic, Contact.
- Covered docs-header Product Page and Changelog links.
