# Portfolio v2 — Tran Chinh Manh

A rebuilt version of the original portfolio, redesigned in the visual language of
[revelatio.studio](https://revelatio.studio): dark canvas, oversized grotesk type,
smooth inertia scrolling and scroll-driven motion.

**The content carried over from the previous portfolio is unchanged — only translated
from Vietnamese into English.** Two capabilities were added at your request: CAD / 3D
design in SOLIDWORKS and FDM 3D printing, together with a new section built around
your speaker enclosure model.

## Folder

```
portfolio-v2/
├── index.html      ← open this file
├── README.md
└── assets/
    ├── avatar.jpg
    ├── 3lop.jpg
    ├── stm32.jpg
    ├── plc.jpg
    ├── led.jpg
    ├── meechabot.jpg
    └── MCR.jpg
```

**Open `index.html` from inside this folder.** `index.html` and `assets/` must stay
together — if you download or move `index.html` on its own, every image and the 3D model
will be missing and the page will look empty. No build step, no server, nothing to install.

## What is in there

**Design**
- Dark-only palette (`#08080a`) with the original orange accent (`#ff6b00`) kept.
- Type: Inter Tight (display), Inter (body), JetBrains Mono (labels) via Google Fonts.
- Floating pill navigation with blur, live Ho Chi Minh City clock, scroll progress bar.

**Skills**
- Each of the six cards carries a thin-stroke technical icon (code brackets, microchip, gear,
  isometric cube, FDM printer, terminal). They draw themselves in with `stroke-dashoffset`,
  staggered card by card, when the grid scrolls into view, and turn orange on hover.
- A small accent square blinks next to each S/0x label, offset in time per card.

**Motion**
- Preloader with a counter and a five-panel curtain reveal.
- Lerp-based inertia smooth scrolling (a self-contained Lenis-style engine, no library).
- Custom cursor: dot + trailing ring, grows on links, switches to a `VIEW` badge over project images.
- Line-by-line masked text reveals, fade/stagger reveals, clip-path wipes, animated rules.
- Two marquees whose speed reacts to scroll velocity and direction.
- Parallax on every project image, plus a curtain that lifts off each one.
- Animated number counters, magnetic buttons, hover fills on the experience rows.
- Full-screen menu with a clip-path wipe on mobile.

**3D Design (section 04)** — deliberately kept small; it reads as a side interest, not a
headline. One compact panel, nothing else.
- A live **wireframe** viewer on WebGL2, built from your STL files. The driver cap and the
  terminal strip are deliberately left out, so five shells are shown.
  It spins on its own, you can drag to rotate it, the **Explode** button takes the assembly
  apart and puts it back together, and hovering one of the small colour swatches isolates
  that part and names it in the corner.
- The lines are not the raw triangle mesh. Each part's **feature edges** are extracted
  offline — edges where the angle between the two faces exceeds 25°, plus open boundaries —
  which is what gives the clean CAD look instead of a noisy triangulated net. 15,705 edges
  in total. Smooth curves correctly show no lines except at their rims.
- Depth test is off and line opacity fades with distance, so the model is see-through the
  way a SOLIDWORKS wireframe is.
- Vertices are quantised to 16-bit and stored in `assets/speaker-model.js` as base64, loaded
  with a plain `<script>` tag so it still works when you open the file straight from disk
  (a `fetch()` of a local binary would be blocked). 204 KB.
- If a browser has no WebGL2, the viewer falls back to `assets/cad-wireframe.jpg`.

**Hero**
- The portrait is rendered live on a `<canvas>` as a **character matrix** — every cell is a
  glyph picked from the ramp `" .,:;i1tfLCG08@"` by brightness, so the picture is literally
  drawn in letters and digits. Cells are ~9px, which is finer than a dot grid and holds much
  more detail. The glyphs are pre-rendered once into a sprite atlas (one sprite per
  brightness level, colour baked in) and blitted with `drawImage`, which keeps ~5,000 cells
  per frame cheap. On first load the characters scramble and settle; the cursor brightens
  the cells around it; the whole field drifts slowly as you scroll. The source image is
  inlined as a data URI so it also works from `file://`.

**Engineering notes**
- Single self-contained `index.html` — no frameworks, no build tooling.
- All images were re-encoded (max 1600px, progressive JPEG): 14.6 MB → 1.7 MB.
- Respects `prefers-reduced-motion`; smooth scroll and the custom cursor are
  disabled on touch devices, where native scrolling takes over.
- Fully responsive down to 390px; semantic landmarks and aria labels preserved.

## Editing

Everything lives in `index.html`, split into commented blocks:
CSS sections 1–19, then HTML sections (hero, 01 about … 07 contact), then JS
sections 1–13. Search for a heading such as `04 / PROJECTS` or `12. HERO DOT-MATRIX`
to jump to the part you want.

To swap the hero portrait you need to change it in two places: `assets/avatar.jpg`
(the fallback image) and the `AVATAR_SRC` data URI in JS section 12.

### The speaker write-up

The two paragraphs describing the enclosure are marked in the HTML with

```html
<!-- EDIT ME: replace the two paragraphs below with your own words -->
```

They only describe what is visible in the model you sent — please replace them with the
real story (why you built it, what you learned, whether it was printed) and correct
anything I got wrong. The `.spec` block just below them holds the four figures
(software, model type, body count, how it is made) and is easy to edit.

To add more images to the gallery, drop them in `assets/` and copy one of the
`<figure class="shot">` blocks in section 04.
