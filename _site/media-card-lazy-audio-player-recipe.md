# Media Card Lazy Audio Player Recipe

This note documents the pattern used on this Jekyll site to turn normal media cards into thumbnail-first audio/player cards.

The goal is:

- Keep the existing `/media/` layout and card design.
- Show the normal media thumbnail first.
- Add a play overlay on top of the thumbnail.
- Only load the third-party player or audio file after the visitor clicks.
- Seek to a specific timestamp when possible.
- Avoid changing unrelated media posts.

This is a static-site pattern. It works on GitHub Pages because it only uses Jekyll, HTML, CSS, and client-side JavaScript.

## Current Site Files

The relevant files are:

```text
collections/_media/*.md
_layouts/media-3.html
_layouts/media.html
_includes/theme/cards/card-post.html
_sass/theme/_custom.scss
assets/images/gen/media/*
```

The active media page is:

```text
pages/media.md
```

It uses:

```yaml
layout: media-3
```

So the main page is rendered by:

```text
_layouts/media-3.html
```

The older alternate media layout is still supported:

```text
_layouts/media.html
```

Both layouts should pass the same media metadata into the card include so behavior stays consistent if layouts are changed later.

## Existing Media Pattern

A normal media item looks like this:

```yaml
---
title: "Beat 102 103"
categories: ["Radio", "Interview"]
description: "Spoke about Blue Monday and what we can do for our mental health in January"
date: 2026-01-18
outlet: Beat 102 103
link: "https://www.beat102103.com/podcasts/the-sunday-grill/alpacas-friend-dating-the-kitchen-edit-sunday-grill-january-18-2026-2226304"
thumbnail: "/assets/images/gen/media/beat.jpg"
---
```

By default, the card include uses:

- `title` as the card title.
- `description` as the card copy on `media-3`.
- `thumbnail` as the card image.
- `link` as the title/image URL.

The card include is:

```text
_includes/theme/cards/card-post.html
```

## Lazy Player Metadata

Lazy media cards are driven by extra front matter fields.

There are two supported cases:

1. RTÉ iframe player.
2. Direct audio file, such as an MP3 from GoLoud.

### RTÉ Iframe Fields

Use these when the provider requires its own iframe player:

```yaml
rte_clip_id: "11788820"
rte_start_seconds: 3331
rte_start_label: "55:31"
```

Meaning:

- `rte_clip_id`: RTÉ Bosco player clip ID.
- `rte_start_seconds`: timestamp converted to seconds.
- `rte_start_label`: human-readable overlay text.

The card renders the thumbnail and this button:

```text
Play RTÉ clip from 55:31
```

On click, it creates this iframe:

```text
https://www.rte.ie/bosco/components/player/iframe.html?radioUI=true&pl_pillar=Radio&clipid=11788820
```

Then it sends:

```js
iframe.contentWindow.postMessage({
  action: "play",
  currentTime: startAtSeconds
}, "https://www.rte.ie");

iframe.contentWindow.postMessage({
  action: "seekto",
  currentTime: startAtSeconds
}, "https://www.rte.ie");
```

### Direct Audio Fields

Use these when there is a direct `.mp3` or `.m4a` URL:

```yaml
audio_src: "https://example.com/episode.mp3"
audio_start_seconds: 1508
audio_start_label: "25:08"
```

Meaning:

- `audio_src`: direct audio file URL, without the `#t=` fragment.
- `audio_start_seconds`: timestamp converted to seconds.
- `audio_start_label`: human-readable overlay text.

The card renders the thumbnail and this button:

```text
Play clip from 25:08
```

On click, it creates a native HTML audio player:

```html
<audio controls preload="metadata" src="https://example.com/episode.mp3#t=1508"></audio>
```

It also sets `currentTime` after metadata loads, because some browsers ignore the `#t=` fragment:

```js
player.addEventListener("loadedmetadata", function () {
  if (startAtSeconds) {
    player.currentTime = startAtSeconds;
  }
}, { once: true });
```

## Current Specific Examples

### RTÉ Radio 1: Divine Sparks

File:

```text
collections/_media/2026-04-10-divine-sparks.md
```

Front matter:

```yaml
---
title: "RTÉ Radio 1"
description: "Radio essay with RTÉ Radio 1 on connecting with nature for Divine Sparks with Áine Lawlor."
date: 2026-04-10
outlet: "RTÉ Radio 1"
link: "https://www.rte.ie/radio/radio1/clips/11788820/"
thumbnail: "/assets/images/gen/media/rte_radio_1.png"
rte_clip_id: "11788820"
rte_start_seconds: 3331
rte_start_label: "55:31"
---
```

Timestamp:

```text
55:31 = 55 * 60 + 31 = 3331
```

Behavior:

- Shows `rte_radio_1.png`.
- Button says `Play RTÉ clip from 55:31`.
- Click creates the RTÉ iframe and seeks to 55:31.

### RTÉ 2FM: Weekend Morning With Katja Mia

File:

```text
collections/_media/2026-02-11-rte_2fm.md
```

Front matter:

```yaml
---
title: "RTÉ 2FM"
categories: ["Consumer", "Interview"]
description: "I spoke to RTÉ 2FM about my reporting on wedding trends for 2026"
date: 2026-02-11
outlet: RTÉ 2FM
link: "https://www.rte.ie/radio/2fm/2fm-weekend-morning-with-katja-mia/2026/0207/1557253-2fm-weekend-morning-with-katja-mia-saturday-7-february-2026//"
thumbnail: "/assets/images/gen/media/rte_2fm.jpg"
rte_clip_id: "11778104"
rte_start_seconds: 5887
rte_start_label: "1:38:07"
---
```

Timestamp:

```text
1:38:07 = 1 * 60 * 60 + 38 * 60 + 7 = 5887
```

Behavior:

- Shows `rte_2fm.jpg`.
- Button says `Play RTÉ clip from 1:38:07`.
- Click creates the RTÉ iframe and seeks to 1:38:07.

### Beat 102 103: GoLoud Direct MP3

File:

```text
collections/_media/2026-01-18-beat-102-103.md
```

Front matter:

```yaml
---
title: "Beat 102 103"
categories: ["Radio", "Interview"]
description: "Spoke about Blue Monday and what we can do for our mental health in January"
date: 2026-01-18
outlet: Beat 102 103
link: "https://www.beat102103.com/podcasts/the-sunday-grill/alpacas-friend-dating-the-kitchen-edit-sunday-grill-january-18-2026-2226304"
thumbnail: "/assets/images/gen/media/beat.jpg"
audio_src: "https://bauernordic-pods.sharp-stream.com/ie/2856/sg_jan_18_2026_e0fb1aa9_normal.mp3?aw_0_1st.episodeid=354347&aw_0_1st.collectionid=2856"
audio_start_seconds: 1508
audio_start_label: "25:08"
---
```

Timestamp:

```text
25:08 = 25 * 60 + 8 = 1508
```

Behavior:

- Shows `beat.jpg`.
- Button says `Play clip from 25:08`.
- Click keeps the thumbnail visible and overlays a native audio control bar near the bottom.
- The audio player seeks to 25:08.

## Layout Integration

The media layouts pass item metadata into the shared card include.

In `_layouts/media-3.html`:

```liquid
{% include theme/cards/card-post.html
  title=item.title
  description=item.description
  thumbnail=item.thumbnail
  url=item.link
  rte_clip_id=item.rte_clip_id
  rte_start_seconds=item.rte_start_seconds
  rte_start_label=item.rte_start_label
  audio_src=item.audio_src
  audio_start_seconds=item.audio_start_seconds
  audio_start_label=item.audio_start_label
  date=item.date
  show_read_more=false
  style="full"
%}
```

In `_layouts/media.html`, keep the same optional fields:

```liquid
{% include theme/cards/card-post.html
  title=item.title
  description=item.outlet
  thumbnail=item.thumbnail
  youtube_id=item.youtube_id
  url=item.link
  rte_clip_id=item.rte_clip_id
  rte_start_seconds=item.rte_start_seconds
  rte_start_label=item.rte_start_label
  audio_src=item.audio_src
  audio_start_seconds=item.audio_start_seconds
  audio_start_label=item.audio_start_label
  date=item.date
  style="row"
%}
```

This keeps the behavior tied to metadata instead of hardcoding a specific post into the layout.

## Card Include Pattern

The card include follows this priority:

1. If `rte_clip_id` exists, render an RTÉ lazy iframe player.
2. Else if `audio_src` exists, render a direct-audio lazy player.
3. Else if `thumbnail` exists, render a normal linked thumbnail.
4. Else if `youtube_id` exists, render YouTube embed behavior.

The top-level class also treats these as thumbnail cards:

```liquid
<div class="card card-post card-{{ style }} {% if thumbnail or youtube_id or rte_clip_id or audio_src %}card-has-thumbnail{% endif %}">
```

## RTÉ Lazy Iframe Pattern

The RTÉ branch starts as thumbnail plus button:

```html
<div class="card-thumbnail card-thumbnail-rte">
  <div
    class="rte-lazy-player"
    data-clip-id="11788820"
    data-start-seconds="3331">
    <button class="rte-lazy-button" type="button">
      <img alt="RTÉ Radio 1" src="/assets/images/gen/media/rte_radio_1.png">
      <span>Play RTÉ clip from 55:31</span>
    </button>
  </div>
</div>
```

On click, JavaScript creates the iframe:

```js
const iframe = document.createElement("iframe");
iframe.src = "https://www.rte.ie/bosco/components/player/iframe.html?radioUI=true&pl_pillar=Radio&clipid=" + encodeURIComponent(clipId);
iframe.allow = "autoplay";
```

After iframe load, it sends play and seek commands:

```js
iframe.contentWindow.postMessage({
  action: "play",
  currentTime: startAtSeconds
}, "https://www.rte.ie");

iframe.contentWindow.postMessage({
  action: "seekto",
  currentTime: startAtSeconds
}, "https://www.rte.ie");
```

This avoids loading RTÉ's consent UI until the visitor explicitly clicks.

## Direct Audio Lazy Pattern

The direct-audio branch starts as thumbnail plus button:

```html
<div class="card-thumbnail card-thumbnail-audio">
  <div
    class="audio-lazy-player"
    data-audio-src="https://example.com/episode.mp3"
    data-start-seconds="1508">
    <button class="audio-lazy-button rte-lazy-button" type="button">
      <img alt="Beat 102 103" src="/assets/images/gen/media/beat.jpg">
      <span>Play clip from 25:08</span>
    </button>
  </div>
</div>
```

On click, JavaScript creates an audio player:

```js
const player = document.createElement("audio");
player.controls = true;
player.preload = "metadata";
player.src = source + (startAtSeconds ? "#t=" + startAtSeconds : "");
```

To avoid a blank/gaping thumbnail area, keep a copy of the image:

```js
const thumbnail = button.querySelector("img");
const playerWrap = document.createElement("div");

playerWrap.className = "audio-player-active";

if (thumbnail) {
  playerWrap.appendChild(thumbnail.cloneNode(true));
}

playerWrap.appendChild(player);
container.replaceChildren(playerWrap);
```

Then seek robustly:

```js
player.addEventListener("loadedmetadata", function () {
  if (startAtSeconds) {
    player.currentTime = startAtSeconds;
  }
}, { once: true });
```

## Styling Pattern

The SCSS belongs in:

```text
_sass/theme/_custom.scss
```

Do not edit generated CSS directly in `_site/assets/css/main.css`.

The thumbnail wrappers need to behave like normal 16:9 media cards:

```scss
.card-thumbnail-rte,
.card-thumbnail-audio {
  display: block;
  width: 100%;
  background: #111;
}
```

The play overlay uses a translucent accent background:

```scss
.rte-lazy-button span {
  background: rgba(236, 37, 90, 0.78);
  border-radius: var(--button-border-radius);
  bottom: 14px;
  color: var(--color-primary-text);
  left: 50%;
  padding: 9px 12px;
  position: absolute;
  transform: translateX(-50%);
  white-space: nowrap;
}
```

The direct audio player keeps the thumbnail and overlays controls:

```scss
.audio-player-active {
  position: relative;
  width: 100%;

  img {
    aspect-ratio: 16 / 9;
    display: block;
    object-fit: contain;
    width: 100%;
  }

  audio {
    background: rgba(17, 17, 17, 0.72);
    border-radius: var(--button-border-radius);
    bottom: 14px;
    left: 50%;
    max-width: calc(100% - 28px);
    position: absolute;
    transform: translateX(-50%);
    width: 92%;
  }
}
```

## Thumbnail Requirements

Use the same visual dimensions as existing media thumbnails.

Current known media thumbnail size:

```text
1248 x 702 px
```

That is a 16:9 aspect ratio.

For example, the RTÉ Radio 1 logo was copied into:

```text
assets/images/gen/media/rte_radio_1.png
```

and resized to:

```text
1248 x 702 px
```

Use image assets under:

```text
assets/images/gen/media/
```

Then reference them from media front matter:

```yaml
thumbnail: "/assets/images/gen/media/example.jpg"
```

## Adding A New Lazy Player Media Post

1. Add or edit a Markdown file under:

```text
collections/_media/
```

2. Keep normal card fields:

```yaml
title: "Outlet Name"
description: "Short description."
date: YYYY-MM-DD
outlet: "Outlet Name"
link: "https://original-page.example"
thumbnail: "/assets/images/gen/media/example.jpg"
```

3. Add only one player type.

For RTÉ:

```yaml
rte_clip_id: "11788820"
rte_start_seconds: 3331
rte_start_label: "55:31"
```

For direct audio:

```yaml
audio_src: "https://example.com/episode.mp3"
audio_start_seconds: 1508
audio_start_label: "25:08"
```

4. Rebuild:

```sh
rbenv exec bundle exec jekyll build
```

5. Check:

```text
http://127.0.0.1:4000/media/
```

## Things To Avoid

- Do not hardcode a single media post into `_layouts/media-3.html`.
- Do not edit `_site/assets/css/main.css` directly; it is generated.
- Do not load third-party iframes immediately unless necessary.
- Do not bypass third-party consent popups.
- Do not use a normal webpage URL as `audio_src`; it must be a direct media file.
- Do not add both `rte_clip_id` and `audio_src` to the same media post unless the card include is intentionally changed to handle that case.

## Verification Checklist

Before committing:

```sh
rbenv exec bundle exec jekyll build
```

Check the generated page for expected markers:

```sh
rg -n "rte-lazy|audio-lazy|Play clip|Play RTÉ clip" _site/media/index.html
```

Check source media files:

```sh
rg -n "rte_clip_id|audio_src" collections/_media
```

Check dimensions for new thumbnails:

```sh
sips -g pixelWidth -g pixelHeight assets/images/gen/media/example.png
```

Expected for current media thumbnails:

```text
pixelWidth: 1248
pixelHeight: 702
```

