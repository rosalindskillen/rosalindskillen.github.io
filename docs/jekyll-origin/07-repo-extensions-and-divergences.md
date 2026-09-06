# 07. Repository Extensions & Major Divergences

> **Context:** This document details the **new architectural structures and systems introduced into this repository that do NOT exist in the upstream Zerostatic Jekyll Origin theme.**

---

## 1. The `media` Collection (`collections/_media/`)

### Why It Was Introduced
The vanilla Zerostatic theme only supports standard written blog posts (`posts`). Because Rosalind Skillen frequently contributes to broadcast media (radio, television, podcasts, panel discussions, and keynotes), a dedicated collection was introduced:
* **Configuration:** Added to [`_config.yml`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_config.yml):
  ```yaml
  collections:
    media:
      output: true
      permalink: /media/:path/
  ```
* **Storage:** Files live in [`collections/_media/`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/collections/_media/) using `YYYY-MM-DD-slug.md` naming.
* **Layouts:** Rendered using [`_layouts/media-3.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/media-3.html) (active 2-column full card layout) and [`_layouts/media.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/media.html) (legacy row layout).

---

## 2. Interactive Lazy Media Players in Cards

### Vanilla vs. This Repository
* **Vanilla Theme:** The card component ([`card-post.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_includes/framework/card-post.html)) only supports a static thumbnail image wrapped in an `<a href="...">` link.
* **This Repository:** The card include was overhauled in [`_includes/theme/cards/card-post.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_includes/theme/cards/card-post.html) to support **three distinct inline lazy-playback engines**:

```
                              ┌─────────────────────────┐
                              │    Front Matter In      │
                              │ collections/_media/*.md │
                              └────────────┬────────────┘
                                           │
          ┌────────────────────────────────┼────────────────────────────────┐
          ▼                                ▼                                ▼
  [rte_clip_id]                     [audio_src]                       [youtube_id]
          │                                │                                │
          ▼                                ▼                                ▼
┌──────────────────┐             ┌──────────────────┐             ┌──────────────────┐
│   RTÉ Lazy Player│             │Native Audio Playr│             │YouTube Lazy Playr│
│ Thumbnail + Play │             │Thumbnail + Play  │             │Thumbnail + Play  │
│  Button Overlay  │             │  Button Overlay  │             │  Button Overlay  │
└─────────┬────────┘             └─────────┬────────┘             └─────────┬────────┘
          │ (On Click)                     │ (On Click)                     │ (On Click)
          ▼                                ▼                                ▼
┌──────────────────┐             ┌──────────────────┐             ┌──────────────────┐
│ Inject RTÉ Bosco │             │ Inject HTML5     │             │ Inject YouTube   │
│ Iframe + postMsg │             │ <audio controls> │             │ 16:9 Iframe with │
│  seekto & play   │             │ with #t=seconds  │             │ ?start=seconds   │
└──────────────────┘             └──────────────────┘             └──────────────────┘
```

### Engine 1: RTÉ Bosco Radio Player
* Used for RTÉ 2FM appearances.
* **Front Matter:**
  ```yaml
  rte_clip_id: "22573616"
  rte_start_seconds: 3331       # Seconds offset
  rte_start_label: "55:31"       # Display label on play button
  ```
* **Client Logic:** Renders a thumbnail with a play badge. When clicked, injects an `iframe` pointing to `https://www.rte.ie/bosco/components/player/iframe.html?radioUI=true&pl_pillar=Radio&clipid={id}`. Once loaded, it uses `postMessage` to trigger `"seekto"` and `"play"`.

### Engine 2: Native Audio Player (`<audio controls>`)
* Used for podcasts (e.g. Newstalk Daily, GoLoud, Beat 102 103).
* **Front Matter:**
  ```yaml
  audio_src: "https://bauernordic-pods.sharp-stream.com/.../clip.mp3"
  audio_start_seconds: 104
  audio_start_label: "1:44"
  ```
* **Client Logic:** Renders a thumbnail button. When clicked, injects an HTML5 `<audio>` player with `#t={seconds}` media fragment, seeks to `audio_start_seconds`, and starts playback.

### Engine 3: Lazy YouTube Embed
* Used for televised segments (Virgin Media Ireland AM) and conference speeches (Green Foundation Ireland, TEDx).
* **Front Matter:**
  ```yaml
  youtube_id: "iq52zCD56lw"
  youtube_start_seconds: 2
  youtube_start_label: "0:02"
  ```
* **Client Logic:** On click, injects a responsive 16:9 `.video-container` containing a YouTube iframe with `autoplay=1&rel=0&start={seconds}`.

---

## 3. Directory Flattening

The upstream Zerostatic theme placed includes deep in subdirectories:
* Vanilla: `_includes/framework/global/cookies/cookie-consent.html`
* Vanilla: `_includes/framework/global/head/seo-meta-tags.html`
* Vanilla: `_includes/framework/global/head/og-meta-tags.html`

In this repository, all framework includes have been moved directly under [`_includes/framework/`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_includes/framework/):
* `_includes/framework/cookie-consent.html`
* `_includes/framework/seo-meta-tags.html`
* `_includes/framework/og-meta-tags.html`

All templates in `_layouts/` and `_includes/` reference these flat paths.

---

## 4. `localStorage` Dark Mode Persistence

* **Vanilla Theme:** Dark mode state was written to `sessionStorage`. Any page reload or new browser tab risked losing the user's preference.
* **This Repository:** Updated in both [`assets/js/darkModeSwitch.js`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/assets/js/darkModeSwitch.js) and the inline head script in [`_layouts/default.html`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_layouts/default.html) to use **`localStorage`**:
  ```javascript
  localStorage.getItem('darkMode') === 'true' && document.documentElement.setAttribute('data-bs-theme', 'dark');
  ```
  This eliminates flash of unstyled theme (FOUT) and persists user choice permanently.

---

## 5. Summary of Architecture Supersession

When developing or modifying this codebase:
1. **For written articles:** Use `collections/_posts/` with `custom_url`.
2. **For broadcast appearances:** Use `collections/_media/` with `rte_clip_id`, `audio_src`, or `youtube_id`.
3. **For include calls:** Always use flat paths (`_includes/framework/<name>.html`).
4. **For styling:** Put project overrides into [`_sass/theme/_custom.scss`](file:///Users/naivedyabansal/Antigravity/Repos/rosalindskillen.github.io/_sass/theme/_custom.scss).
