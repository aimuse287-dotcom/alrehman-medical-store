# Alrehman Medical Store — GitHub Pages deploy package

Deploy this folder as-is to GitHub Pages (free). It is a complete copy of the
live site plus the new real-room 3D Store.

## How to deploy (GitHub Pages)

1. GitHub → New repository (public), e.g. `alrehman-medical-store`.
2. Upload ALL files in this folder to the repository root (keep the
   `vendor/` and `textures/` folders).
3. Settings → Pages → Source: **Deploy from a branch** → Branch: `main`,
   Folder: `/ (root)` → Save.
4. The site goes live at `https://<username>.github.io/alrehman-medical-store/`
   (different URL from the Netlify site — Netlify deploys are blocked:
   credits exhausted for the Nov 10 – Dec 9 cycle).

## What is in here

- `index.html` — the store home page (unchanged from the live V26 site).
  Its **3D Store** button opens `3d-store.html`.
- `3d-store.html` — the 3D Store viewer (Phase 1: look / room + door flow).
- `vendor/three.min.js`, `vendor/OrbitControls.js` — Three.js r128, local
  copies (no CDN needed).
- `textures/tex_back.jpg`, `tex_left.jpg`, `tex_front.jpg` — rack-face
  textures cropped from Sir's own photos/video frames (see below).

## The 3D room — how it was built (no mesh file)

KIRI's mesh build FAILED ("Failed generating the 3D model" from the 54
photos), so there is no OBJ. The room is instead built as **Three.js
geometry from the verified per-wall inventory**
(`rack_inventory_2026-11-23.md`), with real photos used ONLY as
rack-face textures so the shelves look like his real room:

- **Main rack wall (V99, the wall you face on entering): 6 bays, 9 rows.**
  White racks, RED trim, glass shelves, bench below. Texture: real V99
  frame. Teal glass panel in front of bay 6 + diagonal corner shelf above it.
- **Left wall (V38): 5 bays, 9–10 rows** (10 built), bench below, green
  metal chair with beige cloth at the back end. Texture: real morning
  close-up (P08).
- **Front wall (V64, glass frontage):** glass door (entry, right side),
  fixed glass panel, blue metal-grille sliding window, red licences board,
  switch board, white split-AC above the doors, round 3-blade wall fan
  LEFT of the AC, dark curtain tied back, white cord on the glass.
  Upper shelf run above: 11 bays, 2 levels (inventory: 10–12, exact total
  unknown). Texture: real morning close-up (P02).
- **Right wall:** no racks (none seen) — counter + black mini-fridge on a
  wooden pallet.
- **Counters (3 runs):** dark-wood front counter, red-top counter under the
  window, middle glass-front display counter. Desk corner with black office
  chair, west of the door.
- **Ceiling:** textured beige, white diagonal beam, ceiling fan (centre,
  slowly turning), long tube light. Floor: grey marble-pattern tiles.
- **Outside the glass:** road, pavement, white buildings, pole, bushes.

Room size: 12 × 6 m, ceiling 3.6 m (no measurements exist — sensible
proportions from the videos, roughly twice as long as wide).

## Behaviour

- Opens on the glass-door entry: sign **"Medicines"** above the door.
  Click the door → doors slide open → camera flies inside.
- Default camera: just inside the entry (right side), looking at the main
  rack wall. Drag to look around, scroll / pinch to zoom.
- **No text anywhere except the gate sign "Medicines". No people. No
  medicine details in the racks (Phase 1).**
- `?cam=entry|back|left|right|front|top` — camera presets, for checks only
  (skips the entry overlay).

## If the room looks rotated

The room is built to Sir's wall map (entry = right side, main rack wall
opposite it), so no rotation is needed. If a wall ever needs swapping, the
wall builders are the `rackWall(...)` calls and the `frontWall` block in
`3d-store.html`.

## Sources

- Rack counts / details: `~/workspace/work/store_3d_demo/rack_inventory_2026-11-23.md`
- Textures: `~/workspace/work/store_3d_demo/video_new/v99_04s.jpg`,
  `video_new/photos/P08_0917.jpg`, `video_new/photos/P02_0914.jpg`
- Live site this copies: `~/workspace/work/alrehman-v21/netlify-deploy/`
  (V26, https://serene-klepon-4c6db5.netlify.app) — untouched.
