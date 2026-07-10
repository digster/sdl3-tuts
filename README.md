# SDL3 Recipes

A progressive, callback-style **C++ curriculum for SDL3** — delivered as a small,
zero-build static website. **87 recipes** across three tracks take you from opening
a window to a deferred renderer, cascaded shadows, reflective water and GPU-driven
rendering, all using the same loop model so you learn it once and reuse it everywhere.

The front page is [`index.html`](index.html); open it in a browser to start.

## What this is

This repo is the **tutorial half** of an SDL3 starter project. It is plain HTML and
CSS — no build step, no framework, no JavaScript beyond a syntax-highlighting CDN.
Every recipe is a narrated walkthrough plus a complete, copy-pasteable C++ listing,
ordered beginner → advanced within each track.

> **Where's the code that runs?** The recipes are written as drop-in listings for a
> companion **`sdl3-2d` starter template** that holds the actual build harness
> (`examples/`, `Makefile`, `make run-cpp` / `make run-gpu`). That template lives in
> a separate repo — it is **not** included here. This is why some in-page links such
> as `../README.md` and `../../examples/...` point outward by design. See
> [ARCHITECTURE.md](ARCHITECTURE.md) for the full relationship.

## The three tracks

| Track | Rendering API | Recipes | From → to |
|-------|---------------|:-------:|-----------|
| [2D](2d/index.html)   | `SDL_Renderer`              | 28 | Hello Window → Pong → scene stack → pathfinding, Asteroids |
| [3D](3d/index.html)   | `SDL_GPU` + `SDL_shadercross` | 31 | Hello Triangle → lit cube + FPS camera → deferred, SSAO → terrain, cascaded shadows, water |
| [bgfx](bgfx/index.html) | `bgfx` + `shaderc`        | 28 | Hello bgfx → spinning cube → PBR, deferred → HDR, occlusion queries, GPU-driven |

- **2D** uses SDL3's high-level renderer — the gentlest on-ramp.
- **3D** drops one tier to `SDL_GPU` (Metal / Vulkan / D3D12 behind one API), with
  shaders authored once in HLSL and cross-compiled via `SDL_shadercross`.
- **bgfx** steps outside SDL for rendering entirely: SDL3 owns the window and input,
  while [bgfx](https://github.com/bkaradzic/bgfx) drives a graphics-API-agnostic
  renderer, with shaders cross-compiled to every backend via `shaderc`.

## Viewing the site

It's static, so any method works:

```sh
# Recommended — serve from the repo root so relative links resolve cleanly:
python3 -m http.server
# then open http://localhost:8000
```

You can also just open [`index.html`](index.html) directly in a browser. If GitHub
Pages is enabled on this repo it will also be live at
`https://digster.github.io/sdl3-tuts/`.

Code blocks are colourised by [Prism.js](https://prismjs.com/) via a CDN — offline,
before the CDN caches, the code still reads, just in monochrome.

## Running the code

Each recipe's **Complete listing** is a drop-in replacement for a file in the
companion `sdl3-2d` template:

| Track | Replace this file | Build with |
|-------|-------------------|------------|
| 2D   | `examples/cpp/callbacks.cpp`     | `make run-cpp` |
| 3D   | `examples/cpp-gpu/triangle.cpp`  | `make run-gpu` |
| bgfx | the template's bgfx example file  | the template's bgfx target |

Replace the file's contents with the recipe's listing, then build from the template
root. Prerequisites:

- **SDL3** — `brew install sdl3` on macOS (or your platform's package / a source build).
- A **C++ compiler** (Clang / GCC / MSVC).
- For the **bgfx** track only: the additional **`bgfx` + `shaderc`** toolchain.

Again: those build files live in the parent template, not in this repo. Start from
the template's own README for installation and the `make` targets.

## The callback model

SDL3 offers an alternative to the classic `while (running)` game loop: you define
four functions and SDL drives them for you.

```cpp
SDL_AppResult SDL_AppInit(void **appstate, int argc, char **argv);  // set up, allocate AppState
SDL_AppResult SDL_AppEvent(void *appstate, SDL_Event *event);       // one event at a time
SDL_AppResult SDL_AppIterate(void *appstate);                       // one frame
void          SDL_AppQuit(void *appstate, SDL_AppResult result);    // tear down
```

Per-app state lives in a single heap `AppState` struct whose pointer SDL threads
back into every callback — **no globals, no static state**. Every recipe in every
track uses this same skeleton, which is also what keeps the code portable to
platforms (web via Emscripten, iOS) where your program isn't allowed to own the
main loop.

## How each recipe is laid out

- **What you'll learn** — the concepts the recipe introduces.
- **Prerequisites** — earlier recipes you should be comfortable with.
- **Walkthrough** — narrative tutorial, code in small pieces, with the *why* spelled out.
- **Complete listing** — the full file, ready to paste in and run.
- **Try it yourself** — small experiments to lock in the concept before moving on.

## Repo layout

```
.
├── index.html        # Landing page — pick a track
├── style.css         # Shared stylesheet for every page
├── 2d/               # 2D track: index.html + 01..28-*.html
├── 3d/               # 3D track: index.html + 01..31-*.html
├── bgfx/             # bgfx track: index.html + 01..28-*.html
├── memory/           # Dated work-session notes (YYYY-MM-DD.md)
├── PROMPT.md         # Running log of prompts used to build the site
├── ARCHITECTURE.md   # Big-picture structure & contributor conventions
└── LICENSE           # MIT
```

## Tech stack

Plain HTML + a single shared `style.css`. No build, no bundler, no dependencies to
install. The only runtime dependency is the [Prism.js](https://prismjs.com/) CDN for
syntax highlighting, loaded in the order `prism.min.js` → `prism-c` → `prism-cpp`
(C++ highlighting extends the C grammar, so `prism-c` must load first).

## License

[MIT](LICENSE) © 2026 digster.
