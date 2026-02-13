# DRM DASH / HLS Player

A collection of browser-based media players for playing **DRM-protected** and **non-DRM** streams in DASH (`.mpd`) and HLS (`.m3u8`) formats. No server required — everything runs client-side via GitHub Pages or any static host.

---

## Players Overview

| # | File | Engine | Formats | DRM Support | Use Case |
|---|------|--------|---------|-------------|----------|
| 1 | [`index.html`](index.html) | JW Player | DASH, HLS | ClearKey | **Main player** — form + URL-param embed mode |
| 2 | [`DRM-player.html`](DRM-player.html) | JW Player | DASH, HLS | ClearKey | Standalone alternate player |
| 3 | [`embbed.html`](embbed.html) | JW Player | DASH, HLS | ClearKey | Frameless embed (for `<iframe>`) |
| 4 | [`EMB.html`](EMB.html) | JW Player | HLS | None | Quick M3U8 embed player |
| 5 | [`claper.html`](claper.html) | Clappr | HLS | None | HLS player with quality & audio selectors |
| 6 | [`mpdLI.html`](mpdLI.html) | Shaka Player | DASH | Widevine | Widevine DRM with license-server URL |

---

## 1. Main DRM Player — `index.html`

The primary player with a sleek form UI. Supports both interactive input and URL-parameter embedding.

**Features:**
- Play DASH (`.mpd`) and HLS (`.m3u8`) streams
- Optional ClearKey DRM (Key ID + Key)
- Auto-detect stream type from URL extension
- URL-parameter mode for direct embedding
- Glassmorphism dark-themed UI
- Navigation links to other players

### Interactive Mode

Open the page, paste a stream URL, optionally add Key ID / Key, and click **Play**.

### URL-Parameter (Embed) Mode

```
https://your-host/index.html?emmbed-url=<STREAM_URL>&kid=<KEY_ID>&key=<KEY>
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| `emmbed-url` or `embed-url` | Yes | Stream manifest URL |
| `kid` | No | ClearKey Key ID (hex) |
| `key` | No | ClearKey Key (hex) |

**Examples:**

```
# HLS without DRM
?emmbed-url=https://example.com/live/index.m3u8

# HLS with ClearKey
?emmbed-url=https://example.com/stream.m3u8&kid=0cbc4d3b...&key=30528c4e...

# DASH with ClearKey
?emmbed-url=https://example.com/manifest.mpd&kid=06341bf3...&key=1f69fd23...

# DASH without DRM
?emmbed-url=https://cdn.bitmovin.com/content/assets/art-of-motion-dash-hls-progressive/mpds/f08e80da.mpd
```

---

## 2. Standalone DRM Player — `DRM-player.html`

A GitHub-themed variant of the main player. Same capabilities with a different visual style.

- Dark theme matching GitHub's UI
- Validation: warns if only one of Key ID / Key is provided
- Same URL-parameter support as `index.html`

---

## 3. Embed Player — `embbed.html`

Designed to be loaded inside an `<iframe>`. No form, no chrome — just the video.

```html
<iframe
  src="embbed.html?emmbed-url=https://example.com/stream.mpd&kid=abc123&key=def456"
  width="854"
  height="480"
  frameborder="0"
  allowfullscreen>
</iframe>
```

**Features:**
- Loading spinner while player initializes
- Error overlay for missing URLs or load failures
- Zero-margin fullscreen layout
- Supports both DASH and HLS with ClearKey

---

## 4. Quick M3U8 Embed — `EMB.html`

The simplest player — just pass an M3U8 URL and it plays.

```
EMB.html?video=https://example.com/live/index.m3u8
```

| Parameter | Required | Description |
|-----------|----------|-------------|
| `video` | Yes | M3U8 stream URL |

- HLS only
- No DRM support
- Loading spinner + error state
- Ideal for quick live stream embeds

---

## 5. Clappr HLS Player — `claper.html`

A feature-rich HLS player built on [Clappr](https://github.com/clappr/clappr).

**Features:**
- **Quality selector** — switch between available bitrates
- **Audio track selector** — choose audio language/track
- **HLS.js backend** — better HLS compatibility
- Auto-recovery on playback errors (16 attempts)
- DVR support (minimum 60s DVR window)
- Custom seek-bar and button colors
- Enter key to play

**Usage:** Open the page, paste an M3U8 URL into the top bar, press Play.

---

## 6. Widevine DRM Player — `mpdLI.html`

Powered by [Shaka Player](https://github.com/shaka-project/shaka-player), this player handles **Widevine DRM** with a license server URL.

**Features:**
- Shaka Player 4.3.6 with built-in UI controls
- Widevine DRM license server configuration
- Keyboard playback controls
- Custom seek-bar colors
- Browser support detection

**Usage:**
1. Enter the MPD manifest URL
2. Enter the Widevine license server URL
3. Click **Load & Play**

---

## Quick Start

### Option A — GitHub Pages
Fork this repo and enable GitHub Pages on the `main` branch. Your players will be live at:
```
https://<your-username>.github.io/drm-player/
```

### Option B — Local
Serve the files with any static server:
```bash
# Python
python -m http.server 8000

# Node.js
npx serve .

# PHP
php -S localhost:8000
```
Then open `http://localhost:8000` in your browser.

---

## Project Structure

```
drm-player/
├── index.html        # Main DRM player (JWPlayer) — form + embed mode
├── DRM-player.html   # Standalone DRM player (JWPlayer)
├── embbed.html       # Iframe embed player (JWPlayer)
├── EMB.html          # Quick M3U8 embed (JWPlayer)
├── claper.html       # Clappr HLS player with quality/audio selectors
├── mpdLI.html        # Shaka Player for Widevine DRM
├── screenshot.png    # Main player screenshot
├── screenshot2.png   # Widevine player screenshot
├── LICENSE           # License
└── README.md         # This file
```

---

## Technology Stack

| Library | Version | Used In | Purpose |
|---------|---------|---------|---------|
| [JW Player](https://www.jwplayer.com/) | Cloud | index, DRM-player, embbed, EMB | DASH/HLS playback + ClearKey DRM |
| [Clappr](https://github.com/clappr/clappr) | Latest | claper | HLS playback with plugins |
| [HLS.js Playback](https://github.com/clappr/hlsjs-playback) | 1.2.0 | claper | HLS.js backend for Clappr |
| [Shaka Player](https://github.com/shaka-project/shaka-player) | 4.3.6 | mpdLI | DASH + Widevine DRM playback |

---

## Browser Support

| Feature | Chrome | Firefox | Edge | Safari |
|---------|--------|---------|------|--------|
| HLS Playback | Yes | Yes | Yes | Yes |
| DASH Playback | Yes | Yes | Yes | Limited |
| ClearKey DRM | Yes | Yes | Yes | No |
| Widevine DRM | Yes | Yes | Yes | No |

---

## URL Parameter Reference

### `index.html` / `DRM-player.html` / `embbed.html`

| Parameter | Description |
|-----------|-------------|
| `emmbed-url` | Stream URL (`.mpd` or `.m3u8`) |
| `embed-url` | Alias for `emmbed-url` |
| `kid` | ClearKey Key ID (hex string) |
| `key` | ClearKey Key (hex string) |

### `EMB.html`

| Parameter | Description |
|-----------|-------------|
| `video` | M3U8 stream URL |

---

## Acknowledgments

- [JW Player](https://github.com/jwplayer/jwplayer) — Video player for the web
- [Clappr](https://github.com/clappr/clappr) — Extensible media player
- [Shaka Player](https://github.com/shaka-project/shaka-player) — Adaptive streaming by Google

---

## License

See [LICENSE](LICENSE) for details.
