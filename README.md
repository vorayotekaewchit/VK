# VK (2026) — Dither + Music Playlist

## Contact form (privacy)

The Contact modal posts to [Formspree](https://formspree.io) (or any compatible `POST` URL). Your real inbox is configured only in Formspree’s dashboard — **not** in `index.html`, so visitors don’t see your email in the page source.

1. Create a form at Formspree and copy the endpoint (e.g. `https://formspree.io/f/abcxyz`).
2. In `index.html`, set `CONTACT_FORM_ACTION` (near the `SECTIONS` / `const S` block) to that URL.

You do **not** need a WordPress-style “contact form plugin” for this static site: a hosted form backend is enough.

**Note:** Links that still point at GitHub (`github.com/...`, `*.github.io`) use your GitHub username in the URL. Hiding that completely would mean a different GitHub account, org, or custom domain for Pages.

## Music tracks loading (tracks.json)
The Music modal no longer uses a hardcoded track list. On page load, it fetches:

`/audio/tracks.json`

It then builds the playlist UI (`<ul id="plist">`) dynamically.

### Expected JSON shape
`tracks.json` should be a JSON array where each entry looks like:

```json
[
  {
    "title": "Track Title",
    "album": "Album Name",
    "year": 2026,
    "src": "/audio/track-01.mp3"
  }
]
```

Notes:
- `src` is passed directly to the existing `Audio` element (`aud.src = ...`).
- Use paths that work with your server. Common options are:
  - absolute site-root paths: `"/audio/track-01.mp3"`
  - or relative to the site root: `"audio/track-01.mp3"`

### Behavior
- After the playlist renders, the first track is automatically selected/loaded (`loadT(0)`), and the “now playing” text updates.
- If the fetch fails (network error, non-200 response, invalid JSON shape), the playlist shows:
  - `[ NO TRACKS LOADED ]`

## Now-playing display
The player’s “now playing” area is updated like this when tracks change:
1. First line: `title`
2. Second line: `album — year` (falls back to `album` if `year` is missing)

## Player controls
Play/pause, prev/next, progress bar, and volume are unchanged. Only the track data source + playlist rendering were updated.

