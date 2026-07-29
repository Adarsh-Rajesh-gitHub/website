# adarshrajesh.com

A single-page personal site. One file, no build step, no dependencies.

## Editing

Everything lives in `index.html` — content in the `<body>`, styling in the `<style>` block in the `<head>`.

The colors are CSS variables at the top of the stylesheet, so changing the palette means editing five lines:

```css
--bg:      #fefdfa;           /* center column */
--bg-edge: #f4f2ec;           /* margins on either side */
--text:   #1c1c1e;            /* body text */
--muted:  #8e8e93;            /* secondary text */
--border: #e5e5e7;            /* hairline dividers */
--accent: oklch(0.55 0.15 250); /* link hover */
```

To add an experience or project entry, copy one `<li>` block inside the relevant `<ul class="list">`. Each entry has a `.mark` (a logo image, or a single-letter `.mark--mono` box) plus a `.meta` / `.title` / `.desc`.

## Photos

The "Who Am I" section reads from `images/photos/`:

- `photo-1.jpg`, `photo-2.jpg` — the photos in the frame
- `closing.jpg` — the sunset behind the whole section

Only one photo shows at a time. Clicking the frame swaps to the next one and updates both the caption underneath and the `1 / 2` counter. To add a third, drop another `<img>` inside `.frame-btn` with its own `data-caption`:

```html
<img src="images/photos/photo-3.jpg" alt="Short description" data-caption="Shows under the photo.">
```

Photos crop to a 3:4 portrait frame, so any aspect ratio works. The backdrop crops to fill the panel from its center. Keep each file small — resize with:

```bash
# only shrinks if larger; won't upscale a small photo
sips -Z 1600 your-photo.jpg --out images/photos/photo-1.jpg
```

The panel spans the width of the center column, which it gets from a negative margin (`margin: 4rem -1.5rem 0`) cancelling the `.wrap` padding. To make it full-bleed again, swap that for `margin: 4rem calc(50% - 50vw) 0` and add `overflow-x: hidden` to `body`.

Company logos live in `images/logos/`. The note pointing at the backdrop ("Not a stock photo…") and its little SVG arrow are at the bottom of the `.whoami` section in `index.html`. On screens wider than `77em` it sits out in the beige margin off the panel's bottom-right corner, with the arrow mirrored to aim back at the photo; below that there isn't margin to spare, so it tucks inside the panel instead.

## Previewing

Open `index.html` directly in a browser, or serve it locally:

```bash
python3 -m http.server 8000
```

Then visit http://localhost:8000.

## Deploying

Cloudflare Pages, connected to this repo:

1. Push to `main`.
2. Cloudflare dashboard → Compute → Workers & Pages → Create → Pages → Connect to Git.
3. Pick this repo. Leave the build command empty and set the output directory to `/`.
4. Under the project's Custom domains, add `adarshrajesh.com`. DNS is already on Cloudflare, so the records are created for you.

Every push to `main` redeploys.
