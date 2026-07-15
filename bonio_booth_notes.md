# Bonio Booth — Project Notes

A single-file, interactive photo-booth prototype for stakeholder testing.
Everything lives in `index.html` (HTML + CSS + JavaScript in one file).

---

## File structure

```
Bonio-Booth/
├─ index.html              ← the whole app (edit this)
├─ Frames/                 ← your real frame designs (SVG). MUST be committed.
│  ├─ Slot-1.2x6 (3 images)-1.svg   → 2-slot strip
│  ├─ Slot-2.2x6 (3 images).svg     → 3-slot strip
│  ├─ Slot-3.2x6 (4 images).svg     → 4-slot strip
│  ├─ Slot-4.4x6 (1 images).svg     → single photo
│  ├─ Slot-5.4x6 (2 images).svg     → 2 landscape slots
│  └─ Slot-6.4x6 (6 images).svg     → 2×3 grid
├─ README.md
└─ bonio_booth_notes.md    ← this file
```

---

## How to run it

Open the folder in VS Code, then use the **Live Server** extension:
right-click `index.html` → **Open with Live Server**.
It serves on `http://127.0.0.1:5500`.

Why Live Server (not double-clicking the file): the webcam only works on a
**secure page** (https or localhost). Double-clicking opens `file://`, where
the browser blocks the camera. Live Server = localhost = camera works.

The live GitHub Pages link works too (it's https).

---

## How the code is organized (inside index.html)

Look for the `<script>` near the bottom. Key pieces:

- **`FRAMES`** — the list of frame options. Each has: `id`, `name`, the SVG
  `file`, its `W`/`H` (viewBox size), and `slots` (the photo windows as
  `[x, y, width, height]` in the SVG's own units). `shots` is set automatically
  from the number of slots.
- **`FILTERS`** — the filter options (ต้นฉบับ / ขาวดำ / วินเทจ / สดใส), each a CSS filter.
- **`S`** — the "state" object: what the app currently remembers
  (orientation, selected frame, quantity, filter, which shot, etc.).
- **`STEP`** — one function per screen (attract, frame, quantity, pay,
  getready, capture, preview, confirm, printing, collect). Each returns the
  HTML for that screen.
- **`render()`** — draws the current screen. Every screen is
  `scr-main` (centered content) + `scr-cta` (the bottom button zone).
- **`frameArt(frame, heightInEm)`** — draws a frame: loads its SVG as the
  background and drops the photos into the white windows using the slot
  coordinates. This is what makes the strip look real.
- **`AUTO`** — things that happen automatically on a screen (countdowns,
  payment/print simulation, auto-return home).
- **Idle timeout** — auto-resets the booth if nobody touches it. On the
  preview/confirm screens it auto-prints instead of discarding.
- **Stakeholder panel** — the ⚙ button (bottom-right) opens a hidden test
  panel: orientation, paid/free, payment success/fail, print success/fail,
  and idle timeout on/off. Hidden by default so demos look clean.

---

## Making common changes (things to try)

- **Change wording on a screen:** find the screen in `STEP` (e.g. `collect()`)
  and edit the Thai text.
- **Resize a strip:** change the number passed to `frameArt(...)` on that
  screen (bigger number = bigger strip).
- **Add/adjust a frame:** add an entry to `FRAMES` with its SVG file + slot
  rectangles (read the `<rect fill="white">` values from the SVG).
- **Colors:** the theme colors live in the `:root { ... }` block at the top of
  the `<style>` (e.g. `--accent`, `--pink`, `--mint`).
- **Text size feel:** the base font auto-scales in `sizeDevice()`; the type
  scale is `.h1`, `.h2`, `.sub` in the CSS.

Tip: change one small thing, save, refresh, and see what happens. That's the
fastest way to learn.

---

## Saving your work (Git)

You can use the **Source Control** panel in VS Code (branch icon on the left):
type a short message → **Commit** → **Sync / Push**. That's the same as
GitHub Desktop. About a minute later the GitHub Pages link updates.

---

## Working with Claude Code (in VS Code)

- Just describe what you want in plain language; it edits `index.html`.
- It starts fresh each time — give it context: *"This is a photo-booth
  prototype, all in index.html, with frame SVGs in Frames/. Screens are in the
  STEP object; frameArt() renders the strips."*
- **One editor at a time:** don't have Claude Code and the Cowork assistant
  editing `index.html` at the same moment — last save wins.

---

## Gotchas

- Camera needs https or localhost (not `file://`).
- The `Frames/` folder must be committed/pushed, or the frames won't load online.
- Sample faces and the QR image load from the internet (fine on the Pages link).
- The QR is a placeholder for the demo — not a real download link yet.
