---
name: favicon
description: Designs a premium, hand-crafted-looking favicon and full icon set (SVG source, PNG sizes, apple-touch-icon, .ico) through several genuinely distinct concept iterations and a legibility-at-16px critique pass, so the result reads as a confident vector mark rather than shrunken AI clip art. Use when the user asks for a favicon, site icon, browser tab icon, or app icon, or wants an existing one redesigned to look more polished, professional, or "expensive."
---

# favicon

A favicon lives or dies at 16x16 pixels — most of what makes other icon work look "Illustrator-expensive" (fine detail, soft gradients, elaborate composition) actively hurts a favicon. This skill's job is producing a mark confident enough to survive that constraint, then adding restrained polish back in at the sizes where it can actually be seen.

## What "expensive" means here (and what it doesn't)

- A **single, bold silhouette** that reads instantly at 16px — not a miniature illustration.
- **1–3 colors** at small scale. Save gradients, bevels, and subtle depth for the 192px/512px exports only, and only if they don't muddy the silhouette.
- **True vector source** (SVG), never a raster sketch scaled down. The SVG itself should look like it came out of Illustrator: clean anchor points, consistent stroke weights, curves that land on a pixel grid at 16/32.
- Avoid the generic-AI-icon tells: gratuitous drop shadows, literal/cluttered iconography crammed into a tiny circle, mismatched corner radii, gradients used as decoration rather than to model actual light.
- **Negative space is a tool, not empty space.** Some of the strongest marks (Apple, Pinterest's "P", FedEx's arrow) get their premium feel from one confident cut or curve, not more elements.

## Process

1. **Establish the subject.** Get a brand name, initials, or symbol to work from. If run inside a project, check `package.json` name/description, the README, or the site's `<title>` before asking. If it's still unclear, ask one question rather than guessing at a brand identity.

2. **Generate real alternatives, not palette swaps.** Produce 5–8 SVG concepts that differ in visual *strategy*, e.g.:
   - a monogram/letterform treatment
   - an abstract geometric symbol unrelated to the literal name
   - a simplified pictorial mark (one object, heavily reduced)
   - a negative-space trick (two meanings in one shape)
   - a purely geometric abstraction (arc, angle, grid) that just evokes the brand's tone
   Each as a clean 128×128 (or 24×24-unit) viewBox SVG, no text, no more than 3 fills.

3. **Run the legibility check before picking a favorite.** Rasterize every concept at 16px and 32px (see Rendering) and look at them together, actual size, not zoomed in. Cut or revise anything that turns to mud, loses its silhouette, or only makes sense once you already know what it's supposed to be.

4. **Refine the survivor(s).** Take the strongest 1 (or top 2 if the user wants to choose between finalists) through a polish pass:
   - Snap key curves/edges to the pixel grid at 16 and 32px so it doesn't blur.
   - Rebalance optical weight (optically center, not just mathematically — rounded shapes need to be sized slightly larger than sharp ones to look equal).
   - Add finishing *only* where it survives the small size: a restrained gradient, a precise 1px inner highlight, consistent stroke terminals. If a large-size flourish makes the 16px version worse, drop the flourish.

5. **Export the full set and integrate it** (see Deliverables + Integration).

Keep iterating until the 16px render genuinely looks confident — that's the actual finish line, not a fixed iteration count. A skill worth its name here means killing concepts that don't survive step 3, not shipping the first one that looked nice big.

## Rendering (turning the SVG into real pixels)

Try these in order and use whichever is available — don't stall the whole skill on one missing tool:

1. **claude-in-chrome**, if connected: build a minimal HTML page with the SVG sized exactly to the target box (transparent background), screenshot it, crop to bounds. This is the most faithful option since it renders through an actual browser engine — the same thing that will show the favicon.
2. A system SVG rasterizer if already installed (`rsvg-convert`, `cairosvg`, ImageMagick `convert`/`magick`). Check with `command -v` before assuming.
3. Only if neither is available: redraw the same primitives directly with Pillow (`ImageDraw`) at high supersampling (e.g. render at 512px, downsample to target with `LANCZOS`), and tell the user fidelity may drift slightly from the SVG source since it's a manual re-implementation, not a true SVG parse.

The SVG is always the canonical source. Raster files are derived outputs — never hand-edit a PNG independently of the SVG.

## Deliverables

- `favicon.svg` — canonical vector source (works alone as a modern browser favicon)
- `favicon-16.png`, `favicon-32.png`, `favicon-48.png`
- `apple-touch-icon.png` (180×180)
- `icon-192.png`, `icon-512.png` (PWA/Android home-screen icons)
- `favicon.ico` — multi-resolution (16/32/48), packed with Pillow: `Image.open(png48).save('favicon.ico', sizes=[(16,16),(32,32),(48,48)])`

## Integration

If this is running inside a web project: find where icons already live (`public/`, `static/`, `web/public/`, etc.) rather than assuming a path, drop the files there, and add/update the `<head>` link tags:

```html
<link rel="icon" type="image/svg+xml" href="/favicon.svg">
<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32.png">
<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png">
```

If a `manifest.json`/`site.webmanifest` exists, add the 192/512 PNGs to its `icons` array instead of leaving them orphaned.

## Notes

- If the user asks for a specific Claude model (e.g. by a name/version that doesn't exist), don't silently substitute one — say so and use whatever model is actually running this session. Model choice isn't something this skill can pin; the ideation quality comes from following the process above, not from which model executes it.
- Never fabricate brand meaning the user didn't give you — if you invent a symbolic rationale for a concept, keep it plausible and say it's an interpretation, not a fact about the brand.
