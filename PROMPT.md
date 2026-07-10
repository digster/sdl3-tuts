# Prompts

A running log of the prompts used to drive changes in this repo.

---

## 2026-06-13

> Add a new track for using bgfx with sdl3.

Added a third tutorial track (`bgfx/`) — 12 callback-style C++ recipes for
driving the bgfx renderer from SDL3, with the full shaderc cross-backend shader
workflow — and wired it into the landing page, the shared stylesheet, and the
3D-track footer.

---

## 2026-06-19

> Think of some new tuts to add across the tracks.

Added twelve more recipes — four per track — appended after each track's last
recipe, closing the clearest cross-track gaps. **2D (17–20):**
SDL_RenderGeometry Meshes · Sprite Rotation &amp; Flip · Fixed Timestep · Blend
Modes &amp; 2D Lighting. **3D (18–21):** Instanced Rendering · Post-Processing
&amp; Bloom · Alpha Blending &amp; Transparency · Mipmaps &amp; MSAA. **bgfx
(17–20):** Skybox &amp; Cubemaps · Normal Mapping · Shadow Mapping · Transient
&amp; Dynamic Buffers (the bgfx lighting recipes bring it to parity with the 3D
track). Re-wired each track's footer chain, bumped the index and landing-page
counts to 20/21/20, and logged the work in `memory/`.

---

## 2026-06-20

> generate and add a readme for this repo.

Added a root `README.md` (what the site is, the three tracks with accurate
20/21/20 counts, how to view/run, the SDL3 callback model, repo layout, tech
stack, MIT license) and a companion `ARCHITECTURE.md` (zero-build static
sub-site, the parent `sdl3-2d` template relationship that explains the
intentional outward links, page topology, and contributor conventions). No site
pages changed.

---

## 2026-06-26

> Add more tuts across the tracks.

Added 12 more recipes — four per track, append-only — pushing each track into new
advanced territory (the prior cross-track gaps were already closed). **2D (21–24):**
Clipping &amp; Viewports · Camera &amp; Parallax Scrolling · Tweening &amp; Easing ·
Scene/State Stack. **3D (22–25):** Point &amp; Spot Lights · Tone Mapping &amp; Gamma
· Environment Reflections · Deferred Shading (MRT). **bgfx (21–24):** PBR &amp;
Image-Based Lighting · Compute Particles (flow field) · Stencil Reflections &amp;
Outlines · Deferred Rendering (MRT). Re-wired each track's footer `Next →` and
softened the now-mid-track finales; the new last recipe of each track inherited the
cross-track terminator. Bumped every count to 24/25/24 (track indexes, root cards,
hero ledes, "at a glance" lists, the 3D "How to run" ranges → 14–25). Verified
structurally: 24/25/24 files, 852 internal links 0 broken, 0 unescaped
`<`/`>`/`&amp;` in code blocks, Prism `prism → c → cpp` order, threaded footer
chains, plus a browser render spot-check.

---

## 2026-06-26 (follow-up)

> Okay, create a plan for your suggestion. [then] go ahead

Added the two 3D-track recipes flagged at the end of the previous batch:
**26 — Screen-Space Ambient Occlusion** (a fourth pass on the recipe-25 deferred
G-buffer, stored in view space: hemisphere kernel + tiled noise → blur → lit
composite) and **27 — GPU Particles** (the 3D analog of `bgfx/22` — recipe-17 compute
machinery lifted into world space under an MVP camera, with a flow field, a mouse
attractor and additive billboards; recipe 27 ends the track and bridges to the bgfx
particle recipe). Re-wired `3d/25`'s footer + finale, bumped the 3D count to 27
everywhere (track index hero/meta + the `14–27` "How to run" ranges, the root card,
hero lede and "at a glance" list), and logged it. Counts now **24 / 27 / 24**;
verified structurally (878 links 0 broken, escaping, Prism order, threaded chain) plus
a browser spot-check.

---

## 2026-07-10

> add more tuts across the tracks.

Added 12 more recipes — four per track, append-only — the fourth such batch. **2D
(25–28):** Streaming Textures &amp; Pixel Effects · A* Pathfinding · Spatial Hashing ·
Mini Game: Asteroids (a second capstone). **3D (28–31):** a coherent outdoor arc —
Procedural Terrain &amp; Fog · Frustum Culling · Cascaded Shadow Maps · Water &amp;
Planar Reflections. **bgfx (25–28):** HDR &amp; Tone Mapping (twin of 3D/23) · SSAO
(twin of 3D/26, on recipe-24's G-buffer) · Occlusion Queries · GPU-Driven Indirect
Rendering. Re-wired the old terminators (`2d/24`, `3d/27`, `bgfx/24`) — footer
`Next →` re-pointed and finales softened to forward transitions; the new last recipe
of each track (`2d/28`, `3d/31`, `bgfx/28`) took over the cross-track terminator.
Bumped every count to **28 / 31 / 28** (track indexes, root cards, hero ledes, "at a
glance" lists, the 3D "How to run" ranges → 14–31) and refreshed the stale README
(61 → 87; 20/21/20 → 28/31/28) and ARCHITECTURE counts. Verified the bgfx-native
occlusion-query and draw-indirect APIs against docs (context7) before writing those
listings. Verified structurally (counts, links, escaping, Prism order, threaded
chains) plus a browser spot-check.

---

## 2026-07-10 (follow-up)

> Yes, work on the tuning caveats.

Baked three of batch-4's "tune-this-yourself" caveats into robust defaults.
**CSM (`3d/30`):** texel-snap the cascade ortho box (round the light-space origin to
whole shadow-map texels, fold the remainder into the ortho x/y offset — MJP method, no
inverse) so shadows stop crawling as the camera moves; walkthrough + try-it + warn box
updated. **SSAO (`bgfx/26`):** a caps-driven `u_flipY` uniform
(`getCaps()->originBottomLeft`) flips the projected sample UV to match `screenTri`'s
G-buffer orientation, so AO no longer haloes on non-GL backends; note updated and a
cross-reference added to the already-consistent MSL `3d/26`. **Water (`3d/31`):** the
reflection pass now discards real-world `y < 0` geometry (`clipBelow` uniform +
`discard_fragment()`, world-Y passed through the scene VS) so submerged cubes can't leak
into the mirror; lowered the cubes so they bob through the surface and the clip does
visible work. Verified structurally (28/31/28, links, escaping, Prism order, chains) plus
a browser spot-check.
