# Embedding Audio From A Specific Timestamp

This note explains how to create a static/local HTML page that embeds an audio episode and starts playback from a specific timestamp.

There are two main cases:

1. The page exposes a direct audio file, usually `.mp3`.
2. The site only exposes its own web player, so you need to embed that player and ask it to seek.

The GoLoud example used the first approach. The RTÉ examples used the second approach.

## 1. Convert The Timestamp To Seconds

HTML media fragments and JavaScript media APIs usually use seconds.

Examples:

```text
25:08 = 25 * 60 + 8 = 1508 seconds
55:31 = 55 * 60 + 31 = 3331 seconds
1:38:07 = 1 * 60 * 60 + 38 * 60 + 7 = 5887 seconds
```

Use this whenever you need to set a start time.

## 2. Direct Audio File Approach

If you can find a direct audio URL, use a native HTML `<audio>` element.

Direct audio URLs usually look like this:

```text
https://example.com/path/to/audio.mp3
https://cdn.example.com/episode-name.m4a
```

They should not be normal webpage URLs like:

```text
https://www.example.com/episodes/some-episode-page
```

A webpage URL cannot be used directly inside an `<audio>` tag. The `<audio>` tag needs the actual media file.

### Basic HTML

To start a direct MP3 from a timestamp, add `#t=SECONDS` to the audio URL:

```html
<audio
  controls
  src="https://example.com/path/to/audio.mp3#t=1508">
</audio>
```

That starts at `1508` seconds, which is `25:08`.

You can also use `minutes:seconds` syntax:

```html
<audio
  controls
  src="https://example.com/path/to/audio.mp3#t=25:08">
</audio>
```

The seconds form is usually clearer and easier to generate.

### Full Example Page

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Audio From Timestamp</title>
</head>
<body>
  <main>
    <h1>Episode Title</h1>
    <p>This starts from 25:08.</p>

    <audio
      controls
      preload="metadata"
      src="https://example.com/path/to/audio.mp3#t=1508">
      Your browser does not support the audio element.
    </audio>
  </main>
</body>
</html>
```

## 3. Specific Example: GoLoud

For this GoLoud page:

```text
https://www.goloudplayer.com/episodes/alpacas-friend-dating-the-kitche-ZTBmYjFhYTkwNDZlZDk4MjMzYzZkZDA5ZWNlZGU5OTU=
```

the page contained a direct MP3 URL in its rendered data:

```text
https://bauernordic-pods.sharp-stream.com/ie/2856/sg_jan_18_2026_e0fb1aa9_normal.mp3?aw_0_1st.episodeid=354347&aw_0_1st.collectionid=2856
```

The requested start time was `25:08`.

```text
25 * 60 + 8 = 1508
```

So the embedded audio source became:

```html
<audio
  controls
  preload="metadata"
  src="https://bauernordic-pods.sharp-stream.com/ie/2856/sg_jan_18_2026_e0fb1aa9_normal.mp3?aw_0_1st.episodeid=354347&amp;aw_0_1st.collectionid=2856#t=1508">
  Your browser does not support the audio element.
</audio>
```

Important detail: inside HTML, query-string ampersands should be written as `&amp;`.

This is correct in HTML:

```html
?aw_0_1st.episodeid=354347&amp;aw_0_1st.collectionid=2856#t=1508
```

This is the same URL when written outside HTML:

```text
?aw_0_1st.episodeid=354347&aw_0_1st.collectionid=2856#t=1508
```

## 4. Finding The Direct Audio URL

There are a few practical ways to find the real audio URL.

### Browser DevTools

1. Open the episode page in a browser.
2. Open DevTools.
3. Go to the Network tab.
4. Press play on the site’s audio player.
5. Filter for `mp3`, `m4a`, `aac`, `m3u8`, `media`, or `audio`.
6. Copy the actual media request URL.

### Command Line

You can also fetch the page HTML and search it.

```bash
curl -L -o page.html 'https://example.com/episode-page'
rg -n 'mp3|m4a|audio_url|media|m3u8|player|embed' page.html
```

For GoLoud, the page included a `window.__NUXT__` data block with an `audio_url` field.

Relevant pattern:

```text
audio_url="https://...mp3?..."
```

Once you have the MP3 URL, test it:

```bash
curl -I 'https://example.com/path/to/audio.mp3'
```

Good signs:

```text
HTTP/2 200
content-type: audio/mpeg
accept-ranges: bytes
```

`accept-ranges: bytes` is useful because seeking depends on the server being able to serve parts of the file.

## 5. When There Is No Direct MP3

Some sites do not expose a directly embeddable MP3. They may use:

```text
.m3u8 HLS streams
custom iframe players
CDN rules that block hotlinking
JavaScript-only players
```

RTÉ was like this.

The RTÉ page exposed a player iframe:

```html
<iframe
  src="https://www.rte.ie/bosco/components/player/iframe.html?radioUI=true&pl_pillar=Radio&clipid=11788820">
</iframe>
```

But the underlying full episode stream was an HLS playlist:

```text
https://cdn.rasset.ie/hls-vod/audio/.../manifest.m3u8
```

That HLS URL returned `403` unless requested from RTÉ’s player context, so a plain local `<audio>` tag was not reliable.

In that case, the better approach was:

1. Embed RTÉ’s own iframe player.
2. Send the player a `postMessage` command telling it to play or seek.

## 6. Iframe Player Approach

This pattern only works if the iframe player listens for external commands. RTÉ’s Bosco player does listen for messages like:

```js
{
  action: "play",
  currentTime: 3331
}
```

and:

```js
{
  action: "seekto",
  currentTime: 3331
}
```

### Full Example

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>RTÉ Clip From Timestamp</title>
</head>
<body>
  <main>
    <h1>RTÉ Episode</h1>
    <p>This opens the RTÉ player and seeks to 55:31.</p>

    <iframe
      id="rte-player"
      title="RTÉ audio player"
      src="https://www.rte.ie/bosco/components/player/iframe.html?radioUI=true&pl_pillar=Radio&clipid=11788820"
      allow="autoplay"
      style="width: 100%; height: 220px; border: 0;">
    </iframe>

    <button type="button" id="start-button">Play from 55:31</button>
  </main>

  <script>
    const START_AT_SECONDS = 55 * 60 + 31;
    const player = document.getElementById("rte-player");
    const button = document.getElementById("start-button");

    function playFromTimestamp() {
      player.contentWindow.postMessage({
        action: "play",
        currentTime: START_AT_SECONDS
      }, "https://www.rte.ie");

      player.contentWindow.postMessage({
        action: "seekto",
        currentTime: START_AT_SECONDS
      }, "https://www.rte.ie");
    }

    button.addEventListener("click", playFromTimestamp);
  </script>
</body>
</html>
```

The button is needed because browsers usually block autoplay unless the user interacts with the page.

## 7. Specific Example: RTÉ Radio 1

Original page:

```text
https://www.rte.ie/radio/radio1/clips/11788820/
```

RTÉ clip ID:

```text
11788820
```

Requested timestamp:

```text
55:31 = 3331 seconds
```

Iframe:

```html
<iframe
  id="rte-player"
  src="https://www.rte.ie/bosco/components/player/iframe.html?radioUI=true&pl_pillar=Radio&clipid=11788820"
  allow="autoplay">
</iframe>
```

Seek command:

```js
player.contentWindow.postMessage({
  action: "seekto",
  currentTime: 3331
}, "https://www.rte.ie");
```

## 8. Specific Example: RTÉ 2FM

Original page:

```text
https://www.rte.ie/radio/2fm/2fm-weekend-morning-with-katja-mia/2026/0207/1557253-2fm-weekend-morning-with-katja-mia-saturday-7-february-2026//
```

RTÉ clip ID:

```text
11778104
```

Requested timestamp:

```text
1:38:07 = 5887 seconds
```

Iframe:

```html
<iframe
  id="rte-player"
  src="https://www.rte.ie/bosco/components/player/iframe.html?radioUI=true&pl_pillar=Radio&clipid=11778104"
  allow="autoplay">
</iframe>
```

Seek command:

```js
player.contentWindow.postMessage({
  action: "seekto",
  currentTime: 5887
}, "https://www.rte.ie");
```

## 9. Serving The Page Locally

Put the HTML file somewhere local, for example:

```text
/Users/yourname/audio-page.html
```

Then serve that directory with Python:

```bash
cd /Users/yourname
python3 -m http.server 8000
```

Open:

```text
http://localhost:8000/audio-page.html
```

In the examples created here, the server was started from:

```text
/Users/naivedyabansal
```

So these pages were available as:

```text
http://localhost:8000/rte-clip-55-31.html
http://localhost:8000/rte-2fm-katja-1-38-07.html
http://localhost:8000/goloud-alpacas-25-08.html
```

## 10. Browser Compatibility Notes

### Direct MP3

The direct MP3 approach is the most portable:

```html
<audio controls src="episode.mp3#t=1508"></audio>
```

It should work in modern Chrome, Safari, Firefox, and Edge if the server supports byte ranges and CORS/hotlinking does not block the request.

### HLS `.m3u8`

Native HLS support varies:

```html
<audio controls src="https://example.com/manifest.m3u8#t=1508"></audio>
```

This may work in Safari, but generally does not work reliably in Chrome or Firefox without JavaScript libraries such as Hls.js.

### Iframes

Iframe seeking is site-specific.

This kind of code:

```js
iframe.contentWindow.postMessage({
  action: "seekto",
  currentTime: 1508
}, "https://example.com");
```

only works if the embedded player is programmed to accept that message format.

## 11. Minimal Reusable Templates

### Template A: Direct MP3

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Audio From Timestamp</title>
</head>
<body>
  <audio
    controls
    preload="metadata"
    src="AUDIO_FILE_URL#t=START_SECONDS">
  </audio>
</body>
</html>
```

Replace:

```text
AUDIO_FILE_URL -> direct MP3 or M4A URL
START_SECONDS  -> timestamp converted to seconds
```

### Template B: Player Iframe With Seek Button

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Embedded Player From Timestamp</title>
</head>
<body>
  <iframe
    id="player"
    src="PLAYER_IFRAME_URL"
    allow="autoplay"
    style="width: 100%; height: 220px; border: 0;">
  </iframe>

  <button type="button" id="start-button">Play from timestamp</button>

  <script>
    const START_AT_SECONDS = 1508;
    const player = document.getElementById("player");
    const button = document.getElementById("start-button");

    button.addEventListener("click", () => {
      player.contentWindow.postMessage({
        action: "play",
        currentTime: START_AT_SECONDS
      }, "https://provider.example");

      player.contentWindow.postMessage({
        action: "seekto",
        currentTime: START_AT_SECONDS
      }, "https://provider.example");
    });
  </script>
</body>
</html>
```

Replace:

```text
PLAYER_IFRAME_URL         -> provider's iframe embed URL
START_AT_SECONDS          -> timestamp converted to seconds
https://provider.example  -> iframe origin
```

## 12. Practical Decision Tree

Use this order:

1. Look for a direct `.mp3` or `.m4a`.
2. If found, use `<audio src="DIRECT_URL#t=SECONDS">`.
3. If only `.m3u8` is available, check browser support and CDN restrictions.
4. If direct media is blocked, look for an official iframe player.
5. If the iframe player supports `postMessage`, use a seek button.
6. If none of those work, link to the original page and mention the timestamp manually.

## 13. Files Created In This Session

```text
/Users/naivedyabansal/rte-clip-55-31.html
/Users/naivedyabansal/rte-2fm-katja-1-38-07.html
/Users/naivedyabansal/goloud-alpacas-25-08.html
```

Localhost URLs:

```text
http://localhost:8000/rte-clip-55-31.html
http://localhost:8000/rte-2fm-katja-1-38-07.html
http://localhost:8000/goloud-alpacas-25-08.html
```
