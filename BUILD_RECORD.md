# 3D Store viewer — BUILD RECORD

**Date:** Tue 24 Nov 2026, ~02:00–03:15 PKT (deadline: 3D live by 11:00 PKT)
**Package:** `~/workspace/work/alrehman-v21/gh-pages-deploy/`
**Live site copied:** `~/workspace/work/alrehman-v21/netlify-deploy/` (V26 — untouched)

## What changed (pivot)

KIRI's mesh build FAILED ("Failed generating the 3D model" from the 54
photos) — no OBJ exists. The room is therefore built as **Three.js
geometry from the verified per-wall inventory**, not from a mesh file.
Real photos are used ONLY as rack-face textures.

## Files in the package

| File | Size | Note |
|---|---|---|
| `index.html` | 119,470 B | Home page, unchanged from V26 (3D Store button → `3d-store.html`) |
| `3d-store.html` | ~31 KB | NEW viewer — geometry room, door flow, orbit camera |
| `vendor/three.min.js` | 603,445 B | Three.js r128 (local, no CDN) |
| `vendor/OrbitControls.js` | 26,375 B | OrbitControls r128 (local) |
| `textures/tex_back.jpg` | 48,907 B | Crop of V99 frame — real main rack wall |
| `textures/tex_left.jpg` | 43,971 B | Crop of morning photo P08 — real rack close-up |
| `textures/tex_front.jpg` | 53,395 B | Crop of morning photo P02 — real medicine boxes |
| `README.md` | — | Deploy steps + room contents |
| `BUILD_RECORD.md` | — | This file |

## Room contents (per wall — inventory-verified)

- **Main rack wall (V99, faced on entering):** 6 bays, 9 rows, white racks,
  RED trim, glass shelves, bench below; teal glass panel in front of bay 6;
  diagonal corner shelf above bay 6.
- **Left wall (V38):** 5 bays, 10 rows, bench below; green metal chair with
  beige cloth at the back end.
- **Front wall (V64):** glass door (entry, right side, panels slid open),
  fixed glass panel, blue-grille sliding window, red licences board,
  switch board, white split-AC above the doors, round 3-blade wall fan
  LEFT of the AC, dark tied-back curtain, white cord on the glass.
  Upper shelf run: 11 bays, 2 levels (inventory: 10–12, exact total unknown).
- **Right wall:** no racks (none seen) — counter + black mini-fridge on a
  wooden pallet.
- **Counters:** dark-wood front counter, red-top counter under the window,
  middle glass-front display counter; desk corner + black office chair.
- **Ceiling/floor:** textured beige ceiling, white diagonal beam, ceiling
  fan (centre, slowly turning), tube light; grey marble-pattern tile floor.
- **Outside the glass:** road, pavement, white buildings, pole, bushes.
- Room 12 × 6 m, ceiling 3.6 m (no measurements exist — proportions from
  the videos). 507 meshes total. Default camera just inside the entry
  (right side), looking at the main rack wall.

## Verification (all done locally)

### 1. HTTP serve — PASS
`python3 -m http.server 8321` in the package folder; every file 200:
`index.html`, `3d-store.html`, `vendor/three.min.js`,
`vendor/OrbitControls.js`, `textures/tex_back.jpg`, `tex_left.jpg`,
`tex_front.jpg`.

### 2. JS syntax — PASS
`node --check` on the extracted inline app script (25,527 B): OK.

### 3. Headless scene-build test — PASS (jsdom + stubbed WebGLRenderer)
Harness: `/tmp/verify/harness.js`. Zero JS errors in every run. The render
loop ticks (110+ renders in 1.8 s), all 507 meshes build, 0 bad positions.

| Check | Camera | Target | Meshes | Renders | Errors |
|---|---|---|---|---|---|
| `?cam=entry` | [4.2, 1.6, 1.8] | [0, 1.5, -1.5] | 507 | 112 | 0 |
| `?cam=back` | [0, 1.7, -2.2] | [0, 1.5, 3] | 507 | 110 | 0 |
| `?cam=left` | [-4.6, 1.7, 0] | [0, 1.5, 0] | 507 | 111 | 0 |
| `?cam=right` | [4.6, 1.7, 0] | [0, 1.5, 0] | 507 | 111 | 0 |
| `?cam=front` | [0, 1.7, 2.2] | [0, 1.5, -3] | 507 | 112 | 0 |
| `?cam=top` | [4.5, 3.0, 2.5] | [0, 1.0, -1.0] | 507 | 113 | 0 |
| door-click (no `?cam`) | flies to [4.2, 1.6, 1.8] | [0, 1.5, -1.5] | 507 | 154 | 0 |

Door flow verified: click → doors open → overlay fades → camera flies to
home → controls enabled. Room bounds after fixes: x [-6.12, 6.12],
y [-0.03, 3.72], z [-3.12, 3.12] — inside the room shell.
Geometry mix: 343 boxes, 149 planes, 11 cylinders, 1 circle, 1 torus,
2 spheres.

### 4. Bugs found by the harness and fixed
1. **Left-wall rack depth sign** — shelves/uprights/bench were placed
   behind the wall (x -6.2 instead of -5.8). Fixed: uniform `+offset`.
2. **Left-wall glass shelves rotated 90° wrong** — stuck out through the
   wall (room bounds x reached -6.35). Fixed: axis-aware plane dims.
3. **Ceiling beam poked through front/back walls** (8.6 long at 0.5 rad).
   Fixed: 11 long at the room-diagonal angle 0.464 rad.
4. **Left/right camera presets exceeded OrbitControls maxDistance** (11 >
   8.5) and were clamped. Fixed: presets moved inside the room.
5. **Front teal wainscot crossed the doorway.** Fixed: split into two
   pieces leaving the door opening clear.

### 5. Pixel screenshot — NOT DONE (blocked, see below)
- Direct headless Chrome (`/opt/meta-chromium/chrome`) refuses to start:
  "Hatch WebGL domain policy is required for browser startup" — a policy
  gate on direct launches. Not bypassed.
- `headless-gl` npm install fails (node-gyp native build error on this VM).
- The scene-build test above (507 meshes, correct bounds, correct cameras,
  zero errors, render loop ticking) is the verification done. **A visual
  check is still needed on the deployed GitHub Pages URL** (open
  `3d-store.html` in a real browser, click the door, drag around) before
  calling the 11 AM deadline met.

## Known limits (honest)

- Textures come from 576 px video frames / 720 px photos — they will
  soften on close zoom. The room still reads as his real room at normal
  viewing distance.
- Front upper run built as 11 bays (inventory: 10–12, exact total unknown).
- Left wall built as 5 bays (6th bay unproven — back corner never filmed).
- Right wall has no racks (absence not proven — never filmed face-on).
- No real measurements exist; proportions are estimated from the videos.
- The viewer needs no OBJ — nothing is missing for deploy. If a real mesh
  ever arrives, it is NOT needed by this build.
