# 03. Logo, Header & Dark Mode

> **Upstream Sources:**
> * [https://www.zerostatic.io/docs/jekyll-origin/config/logo/](https://www.zerostatic.io/docs/jekyll-origin/config/logo/)
> * [https://www.zerostatic.io/docs/jekyll-origin/config/header/](https://www.zerostatic.io/docs/jekyll-origin/config/header/)
> * [https://www.zerostatic.io/docs/jekyll-origin/config/darkmode/](https://www.zerostatic.io/docs/jekyll-origin/config/darkmode/)

---

## 1. Upstream Documentation (Vanilla Specification)

### Logo Configuration
Controls branding across desktop, mobile, light mode, and dark mode:
```yaml
logo:
  logo_text: "Jekyll Origin"
  logo_image: assets/images/logo/logo.png
  logo_image_mobile: assets/images/logo/logo-mobile.png
  logo_image_invert: assets/images/logo/logo-invert.png
  logo_image_invert_mobile: assets/images/logo/logo-invert-mobile.png
  logo_image_desktop_height: 30
  logo_image_desktop_width: 30
  logo_image_mobile_height: 28
  logo_image_mobile_width: 28
  show_logo_image_on_mobile: true
  show_logo_image_on_desktop: true
  show_logo_text_on_mobile: false
  show_logo_text_on_desktop: true
```
* **Image Logo:** Expects 4 default PNG assets in `assets/images/logo/`.
* **Text Logo:** Can toggle text on desktop/mobile independently.

### Header Configuration
```yaml
header:
  fixed: false
```
* `fixed: false`: Static header scrolling with the page.
* `fixed: true`: Sticky header with scroll-based background color transition (managed via `assets/js/header.js`).

### Dark Mode
```yaml
darkmode:
  enable_dark_mode: true
  show_dark_mode_toggle_in_header: true
  show_dark_mode_toggle_in_bottom: true
  show_dark_mode_toggle_in_mobile_menu: false
```
* Supports OS-level `prefers-color-scheme`.
* Upstream docs state that manual toggle choices are saved in **`sessionStorage`** (resets when the browser session ends).

---

## 2. Repository Implementation & What Supersedes Vanilla

### Status: SUPERSEDED & CUSTOMIZED
* **SVG Logos Supersede PNGs:** Instead of the default `.png` files, [`_config.yml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_config.yml) points to scalable SVG vector files in [`assets/images/logo/`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/assets/images/logo/):
  * `assets/images/logo/logo.svg`
  * `assets/images/logo/logo-mobile.svg`
  * `assets/images/logo/logo-invert.svg`
  * `assets/images/logo/logo-invert-mobile.svg`
* **`localStorage` Supersedes `sessionStorage`:**
  * Upstream documentation states dark mode uses `sessionStorage`.
  * **In this repository, dark mode uses `localStorage`** in both [`assets/js/darkModeSwitch.js`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/assets/js/darkModeSwitch.js) and the inline early `<script>` in [`_layouts/default.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/default.html):
    ```javascript
    localStorage.getItem('darkMode') === 'true' && document.documentElement.setAttribute('data-bs-theme', 'dark');
    ```
  * *Impact:* User preference persists across browser restarts without flashing white on page refresh.
* **Header Position:** Configured with `header.fixed: false` (static header).
* **Toggle Placement:** Header toggle is active; footer toggle is disabled because the footer itself is disabled.
