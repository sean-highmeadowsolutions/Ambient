# Ambient
Screensaver for Rambox

A single-file HTML "screensaver" page — rotating high-resolution photography with a minimal clock overlay. Designed to fill an empty window in tools like [Rambox](https://rambox.app/), [Station](https://getstation.com/), or any browser tab you'd rather not leave blank.

No build step, no dependencies, no tracking. Just drop `ambient.html` on any static host.

## Features

- Fullscreen rotating photos from [Lorem Picsum](https://picsum.photos)
- Elegant serif clock and date (toggle-able)
- Subtle Ken Burns drift on each image; slow crossfade between
- Film grain and vignette for atmosphere
- Hover-only controls — stays uncluttered when ambient
- Configurable via settings panel **or** URL parameters
- Keyboard shortcuts for everything
- Graceful fallback to generated gradients if Picsum is unreachable
- ~15 KB, zero external JS dependencies (uses Google Fonts only)

## Quick start

Open `ambient.html` directly in a browser, or host it anywhere that serves static files. That's the whole setup.

```bash
# serve locally on :8787
npx http-server -p 8787
# or
python -m http.server 8787
```

Then visit `http://localhost:8787/ambient.html`.

## URL parameters

Since browser storage isn't used, configuration persists via URL. Bookmark your preferred combination.

| Param      | Values                                                       | Default  |
| ---------- | ------------------------------------------------------------ | -------- |
| `source`   | `picsum`, `nature`, `architecture`, `abstract`, `gradient`   | `picsum` |
| `interval` | seconds between rotations                                    | `600`    |
| `clock`    | `1` to show clock, `0` to hide                               | `1`      |
| `gray`     | `1` for grayscale photos, `0` for color                      | `0`      |

Example:

```
ambient.html?source=architecture&interval=1800&gray=1
```

## Keyboard shortcuts

| Key          | Action        |
| ------------ | ------------- |
| `N` / `→`    | Next image    |
| `P` / `←`    | Previous image|
| `Space`      | Pause/resume  |
| `C`          | Toggle clock  |
| `S`          | Toggle settings panel |

## Sources

**Picsum (random)** — a fresh random image from Lorem Picsum every cycle. Great variety, occasional clunker.

**Nature / Architecture / Abstract** — curated Picsum seeds. Each seed is stable (same seed → same photo), so these cycle through a hand-picked set defined in the script. Edit the `SEEDS` object to curate your own.

**Gradients** — zero-network zen mode. Ten curated color triads, rotated through with shifting angles. Useful as a fallback or for distraction-free workspaces.

## Using with Rambox

1. Host `ambient.html` somewhere reachable (see [Deployment](#deployment))
2. In Rambox: **Add App → Custom**
3. Name it whatever you like, paste the URL, pick an icon
4. Pin the app so it stays loaded across Rambox restarts

The page uses no persistent state, so it's safe to let Rambox kill and restore it at will.

## Deployment

Any static host works. Some easy options:

- **Netlify Drop** — drag `ambient.html` onto [app.netlify.com/drop](https://app.netlify.com/drop)
- **GitHub Pages** — push to a repo, enable Pages in Settings
- **Cloudflare Pages**, **Vercel**, **Surge.sh** — all trivial for a single HTML file
- **Local** — `npx http-server` or `python -m http.server`, good for personal use

## Customization

Everything meaningful lives in `<script>` at the bottom of `ambient.html`.

**Add your own curated seeds**: extend the `SEEDS` object. Pick words that produce photos you like, then pin them by name.

```js
const SEEDS = {
  nature: ['forest-canopy','redwood-mist', /* ... */],
  mine:   ['kyoto-rain','brooklyn-dusk','patagonia'],
};
```

Add the key to the `<select id="sourceSelect">` options in the HTML so it shows up in the settings panel.

**Change the image source entirely**: replace the `sourceFor(i)` function. It returns `{ type: 'image', value: url }` or `{ type: 'gradient', value: cssGradient }`. Wire it up to Unsplash (requires API key), NASA APOD, a Dropbox public folder, or any image URL list.

**Adjust the aesthetic**: clock font is [Fraunces](https://fonts.google.com/specimen/Fraunces) in the CSS `@import`. The Ken Burns motion is in the `@keyframes kenburns` block. Crossfade duration is the `transition` on `.slide`.

## Browser support

Modern evergreen browsers. Uses `backdrop-filter`, CSS variable font features, and `IntersectionObserver` indirectly via `requestAnimationFrame`. Tested in Chrome, Edge, and Electron-based wrappers (Rambox, Station).

## Credits

- Photography: [Lorem Picsum](https://picsum.photos) (photos sourced from Unsplash)
- Typefaces: [Fraunces](https://fonts.google.com/specimen/Fraunces) and [DM Sans](https://fonts.google.com/specimen/DM+Sans) via Google Fonts
