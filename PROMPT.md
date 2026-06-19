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
