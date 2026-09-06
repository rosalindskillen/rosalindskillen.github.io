# 06. Pages, Posts & Layouts

> **Upstream Sources:**
> * [https://www.zerostatic.io/docs/jekyll-origin/pages/home/](https://www.zerostatic.io/docs/jekyll-origin/pages/home/)
> * [https://www.zerostatic.io/docs/jekyll-origin/pages/blog/](https://www.zerostatic.io/docs/jekyll-origin/pages/blog/)
> * [https://www.zerostatic.io/docs/jekyll-origin/pages/post/](https://www.zerostatic.io/docs/jekyll-origin/pages/post/)
> * [https://www.zerostatic.io/docs/jekyll-origin/pages/about/](https://www.zerostatic.io/docs/jekyll-origin/pages/about/)
> * [https://www.zerostatic.io/docs/jekyll-origin/pages/contact/](https://www.zerostatic.io/docs/jekyll-origin/pages/contact/)

---

## 1. Upstream Documentation (Vanilla Specification)

### 1. Home Page (`layout: home`)
Primary file: `pages/home.md`.
* Renders hero section with title, description, and social media.
* Renders a configurable grid of recent blog posts:
  ```yaml
  posts:
    heading: "Recent Posts"
    limit: 6
    sort: date # date | weight
    view_more_button_text: "More Posts"
    view_more_button_link: /blog
    columns: 3 # 1 | 2 | 3 | 4
    show_authors: true
    show_categories: false
  ```

### 2. Blog Index (`layout: blog` / `blog-2` / `blog-3`)
Primary file: `blog/index.html`.
* Must be an HTML file (not Markdown) to support `jekyll-paginate`.
* Supports 3 distinct layouts:
  * `blog`: 3-column card grid.
  * `blog-2`: 1-column horizontal row cards.
  * `blog-3`: 2-column large feature cards.

### 3. Post Collection (`layout: post` / `post-2` / `post-3`)
Location: `collections/_posts/YYYY-MM-DD-title.md`.
* Standard Markdown blog posts.
* Three layout options:
  * `post`: Standard blog post with header image.
  * `post-2`: Image-first layout.
  * `post-3`: Split 2-column image and text layout.
* Supports authors mapped from `_data/authors.yml`, categories, and comments toggle.

### 4. About & Basic Pages (`layout: basic`)
Primary file: `pages/about.md`.
* Clean layout for standard narrative pages (title, description, optional image, Markdown body).

### 5. Contact Page (`layout: contact`)
Primary file: `pages/contact.md`.
* Displays the active contact form (Netlify or Formspree) alongside contact information from `_data/contact.yml`.

---

## 2. Repository Implementation & What Supersedes Vanilla

### Status: EXPANDED & RE-ARCHITECTED
* **External Article Cards Supersede Internal Posts:**
  * In the vanilla theme, posts are expected to be full articles rendered at `/blog/:path/`.
  * **In this repo**, Rosalind's work is published across external journalism platforms (*BBC, The Sunday Times, Irish Farmers Journal, Belfast Telegraph*).
  * Posts in [`collections/_posts/`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/collections/_posts/) use a **`custom_url`** front-matter attribute:
    ```yaml
    custom_url: "https://farmersjournal.ie/journalists/rskillen"
    ```
  * In [`_layouts/home.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/home.html) and card includes, this supersedes the internal link, directing visitors straight to the external journalism outlet.
* **Homepage Configuration:**
  * Hero image is set to [`assets/images/rosalind-skillen-homepage-hero.png`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/assets/images/rosalind-skillen-homepage-hero.png).
  * Limits recent post cards to 9, sorted by date.
* **Collection Directory:**
  * Upstream documentation inconsistently mentions `_collections/posts` and `collections/_posts`.
  * In this repository, `collections_dir: collections` is strictly configured in `_config.yml`, making **`collections/_posts/`** the authoritative post location.
* **Stock Category Mismatch:**
  * The markdown files in [`categories/`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/categories/) (`branding.md`, `design.md`, etc.) are vanilla theme leftovers.
  * The actual posts use journalism categories like `Sustainability`, `Health`, `Environment`, `Politics`.
