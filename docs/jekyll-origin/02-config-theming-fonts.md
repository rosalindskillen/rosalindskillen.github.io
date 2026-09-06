# 02. Theme Colors & Fonts

> **Upstream Source:** [https://www.zerostatic.io/docs/jekyll-origin/config/theme-color-font/](https://www.zerostatic.io/docs/jekyll-origin/config/theme-color-font/)

---

## 1. Upstream Documentation (Vanilla Specification)

Jekyll Origin uses centralized variables in [`_config.yml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_config.yml) for brand colors and typography.

### Colors
Colors are set under the `colors` block for both light mode and dark mode:
```yaml
colors:
  # Light-mode colors
  primary_bg: "#EC255A"
  primary_bg_2: "#eed2d9"
  primary_text: "#f9fafb"
  base_bg: "#ffffff"
  base_bg_2: "#ebeef0"
  base_bg_3: "#f1f3f4"
  base_text: "#191a1a"
  base_text_2: "#555555"
  logo_text: "#191a1a"
  header_text: "#191a1a"

  # Dark-mode colors
  primary_bg_dark: "#EC255A"
  primary_bg_2_dark: "#eed2d9"
  primary_text_dark: "#f9fafb"
  base_bg_dark: "#121418"
  base_bg_2_dark: "#1d2026"
  base_bg_3_dark: "#24272d"
  base_text_dark: "#F4F4F5"
  base_text_2_dark: "#D1D5DB"
  logo_text_dark: "#F4F4F5"
  header_text_dark: "#F4F4F5"
```

### Fonts
The theme supports both Google Fonts and self-hosted fonts. Self-hosted fonts are configured by default for privacy and performance.

#### Google Fonts Option
1. Select fonts on [Google Fonts](https://fonts.google.com/).
2. Copy the CSS stylesheet URL.
3. In `_config.yml`:
   ```yaml
   fonts:
     use_google_fonts: true
     google_fonts_url: "https://fonts.googleapis.com/css2?family=..."
     use_self_hosted_fonts: false
     heading: "Montserrat"
     base: "Open Sans"
     monospace: "Fira Mono"
     logo: "Montserrat"
   ```

#### Self-Hosted Fonts Option
1. Download font files (e.g., using [google-webfonts-helper](https://gwfh.mranftl.com/fonts)).
2. Place `.woff2` files in `assets/fonts/<font-name>/`.
3. Add `@font-face` declarations into `assets/css/fonts.css`.
4. In `_config.yml`:
   ```yaml
   fonts:
     use_google_fonts: false
     google_fonts_url: ""
     use_self_hosted_fonts: true
     heading: "Schibsted Grotesk"
     base: "Open Sans"
     monospace: "Fira Mono"
     logo: "Schibsted Grotesk"
   ```

---

## 2. Repository Implementation & What Supersedes Vanilla

### Status: FULLY IMPLEMENTED (CUSTOMIZED ASSETS)
* **Active Palette:** Rosalind Skillen's site uses `#EC255A` (vibrant magenta/coral pink) as the primary brand color, paired with clean light/dark charcoal and zinc backgrounds.
* **Self-Hosted Font Assets:** Self-hosted fonts are active (`use_self_hosted_fonts: true`). Font files are present in [`assets/fonts/`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/assets/fonts/):
  * `fira-mono/`
  * `open-sans/`
  * `schibsted-grotesk/`
  * `font-awesome/`
* **CSS Variable Injection:** In [`assets/css/main.scss`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/assets/css/main.scss), values from `_config.yml` are translated into CSS custom properties (`--color-primary-bg`, `--font-family-base`, etc.) on `:root` and `html[data-bs-theme='dark']`.
* **Custom Styling Overrides:** Additional custom typography styling (e.g. underline thickness, font weights) is superseded by [`_sass/theme/_custom.scss`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_sass/theme/_custom.scss).
