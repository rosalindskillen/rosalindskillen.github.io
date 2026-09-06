# 05. Menus, Footer & SEO Metadata

> **Upstream Sources:**
> * [https://www.zerostatic.io/docs/jekyll-origin/config/main-menu/](https://www.zerostatic.io/docs/jekyll-origin/config/main-menu/)
> * [https://www.zerostatic.io/docs/jekyll-origin/config/footer/](https://www.zerostatic.io/docs/jekyll-origin/config/footer/)
> * [https://www.zerostatic.io/docs/jekyll-origin/config/seo-meta-tags/](https://www.zerostatic.io/docs/jekyll-origin/config/seo-meta-tags/)
> * [https://www.zerostatic.io/docs/jekyll-origin/config/og-meta-tags/](https://www.zerostatic.io/docs/jekyll-origin/config/og-meta-tags/)

---

## 1. Upstream Documentation (Vanilla Specification)

### Main Menu (`_data/menu.yml`)
Configures navigation items for desktop and the responsive mobile drawer:
```yaml
main:
  - name: Home
    url: /
    weight: 1
  - name: Blog
    url: /blog/
    weight: 2
```
* Supports child dropdown menus with `child` arrays, icons, and descriptions.
* Mobile menu behavior is toggled via `menu.show_dropdown_items_in_mobile_menu` and `menu.show_social_media_in_mobile_menu`.

### Footer & Bottom Bar
```yaml
footer:
  enable_footer: true
  footer_title: "Origin"
  enable_social_media_icons: true
  enable_menu_footer_primary: true
  enable_menu_footer_secondary: true
  enable_menu_footer_tertiary: true

bottom:
  enable_bottom: true
  enable_bottom_menu: true
  copyright_text: "© 2023 Zerostatic"
  show_rss_icon: true
```
* Reads column menus from `_data/menu.yml` under keys `footer_primary`, `footer_secondary`, `footer_tertiary`, and `bottom`.

### SEO & Open Graph Tags
* Managed via front matter overrides:
  * `title`, `description`, `image` (page body defaults).
  * `meta_title`, `meta_description`, `meta_image` (search engine & social preview overrides).
* Global Open Graph settings in `_config.yml`:
  ```yaml
  open_graph:
    meta_opengraph_type: "website"
    meta_twitter_card: "summary"
    meta_twitter_site: "@zerostaticio"
    meta_twitter_creator: "@zerostaticio"
  ```

---

## 2. Repository Implementation & What Supersedes Vanilla

### Status: MODIFIED & SUPERSEDED
* **Active Main Navigation:** In [`_data/menu.yml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_data/menu.yml), the navigation is simplified to four items:
  1. Home (`/`)
  2. Media (`/media/`)
  3. About (`/about/`)
  4. Contact (`/contact/`)
  *(Note: The `/blog/` page exists in the repo but is not currently linked in the primary menu).*
* **Footer & Bottom Bar Disabled:**
  * [`_config.yml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_config.yml) explicitly sets `enable_footer: false` and `enable_bottom: false`.
  * The footer menus documented in the theme are completely inactive on Rosalind's site.
* **Flattened SEO & OG Include Paths:**
  * Upstream docs point to `_includes/framework/global/head/seo-meta-tags.html` and `og-meta-tags.html`.
  * **In this repo, they live flat:** [`_includes/framework/seo-meta-tags.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_includes/framework/seo-meta-tags.html) and [`_includes/framework/og-meta-tags.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_includes/framework/og-meta-tags.html).
* **Stale Social Handles:**
  * `meta_twitter_site` and `meta_twitter_creator` in `_config.yml` still point to `@zerostaticio`.
  * `url` in `_config.yml` still points to `https://jekyll-origin.netlify.app`. These should be updated to Rosalind's real domain and handles for production SEO previews.
