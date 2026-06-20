# Architecture

This document explains the "big picture" of the **SDL3 Recipes** site — how the
pages fit together and the conventions to follow when extending it. For what the
site teaches and how to view/run it, see the [README](README.md).

## What kind of project this is

A **zero-build static website**. There is no compiler, bundler, package manager, or
server-side code. The deliverable *is* the set of `.html` files plus one shared
`style.css`. The front page is `index.html` — opening it in a browser (or serving
the folder over HTTP) is the entire "run" step.

The only runtime dependency is the [Prism.js](https://prismjs.com/) CDN used for
code highlighting.

## The parent-template relationship (read this first)

This repo is the **tutorial half** of a larger `sdl3-2d` starter template. The
*other* half — the one that actually compiles and runs — is **not in this repo**:

- `examples/cpp/`, `examples/cpp-gpu/`, the bgfx example, the `gfx.hpp`/`gfx.cpp`
  helpers, the `Makefile` (`make run-cpp`, `make run-gpu`), and the project's own
  README all live one directory level **up**, in the parent template.

This is the single most important thing to understand about the codebase, because
it explains links that look broken but are intentional:

| Link seen in pages | Resolves to | Status |
|--------------------|-------------|--------|
| `../README.md`                       | parent template's README | intentional outward link |
| `../../examples/cpp/callbacks.cpp`   | parent template's 2D example | intentional outward link |
| `../../examples/cpp-gpu/triangle.cpp`| parent template's 3D example | intentional outward link |
| `../index.html`, `2d/index.html`, etc. | within this repo | must always resolve |

When verifying link integrity, treat the `../` / `../../` references to
`examples/` and the parent `README.md` as **expected dangling** links; everything
*inside* this repo must resolve.

## Page topology

```
index.html  (landing — three track cards)
  ├── 2d/index.html    → 2d/01-*.html … 2d/20-*.html
  ├── 3d/index.html    → 3d/01-*.html … 3d/21-*.html
  └── bgfx/index.html  → bgfx/01-*.html … bgfx/20-*.html
```

- **Root → track → recipe.** `index.html` links to the three track index pages;
  each track index lists its recipes; each recipe is a standalone page.
- **Footer "Next →" chains.** Within a track, every recipe's footer threads to the
  next one. The *last* recipe of a track carries a **cross-track terminator** that
  hops to the next track, forming a single 2D → 3D → bgfx reading order.
- **Shared styling.** Every page links the one `style.css` (root pages as
  `style.css`, track/recipe pages as `../style.css`). Recipe pages additionally
  include the Prism CDN CSS + scripts.

Representative files to read before editing:
[`index.html`](index.html) (landing structure),
[`2d/index.html`](2d/index.html) (track-index + `.recipe-list` markup and badges),
[`2d/01-hello-window.html`](2d/01-hello-window.html) (recipe page structure and the
What-you'll-learn / Walkthrough / Complete-listing / Try-it-yourself sections).

## Conventions a contributor must follow

These are project-specific and easy to get wrong:

- **Append, don't renumber.** New recipes are added *after* a track's current last
  recipe. Only the previous last recipe's footer `Next →` and its "end of track"
  closing paragraph get re-wired; existing recipe numbers never shift.
- **Self-contained listings, no external assets.** Each recipe's Complete listing
  must compile as a single drop-in file. Geometry and textures are generated
  procedurally in code rather than loaded from asset files.
- **Prism script load order.** Always include the highlighter scripts as
  `prism.min.js` → `prism-c.min.js` → `prism-cpp.min.js`. `prism-cpp` *extends* the
  `c` grammar, so omitting `prism-c` throws
  `Cannot set properties of undefined (setting 'class-name')` and silently disables
  C++ highlighting. This bug exists on some older 2D/3D pages — see
  [`memory/2026-06-13.md`](memory/2026-06-13.md). New pages must not repeat it.
- **HTML-escape code blocks.** Inside `<pre><code>` use `&lt;` / `&gt;` / `&amp;`
  for `<` / `>` / `&` — bare angle brackets in C++ templates/headers will otherwise
  break the markup. (A site-wide check for unescaped characters is part of the
  verification routine; see the memory notes.)
- **Keep the counts in sync.** When adding or removing recipes, update *all* the
  places that state a count: the `<title>`/`<meta>` and hero text on
  [`index.html`](index.html), the per-track card metas, the "at a glance" lists,
  and the affected track's `index.html` hero + `.recipe-list`.
- **Difficulty badges.** Recipes carry `badge-beginner` / `badge-intermediate` /
  `badge-advanced` classes; keep the badge on the track index consistent with the
  one on the recipe page.

## Working-notes workflow

Two non-page artifacts document how the site was built:

- **`memory/YYYY-MM-DD.md`** — a dated log per work session: the goal, decisions
  made, files touched, technical notes (often API details verified against SDL3 /
  bgfx docs), and verification performed. Read the most recent entries to pick up
  context before extending the site.
- **`PROMPT.md`** — a running log of the prompts that drove each change.

## Verification routine

Because there is no test suite, correctness is checked structurally:

1. **Counts** — `ls 2d/[0-9]*.html 3d/[0-9]*.html bgfx/[0-9]*.html` must match the
   numbers stated across the index pages (currently 20 / 21 / 20).
2. **Links** — every *intra-repo* link resolves; only the documented `../` /
   `../../` parent-template references may dangle.
3. **Code blocks** — no unescaped `<` / `>` / `&` inside any `<pre><code>`, and the
   three Prism scripts present in `prism → c → cpp` order on every recipe page.
4. **Navigation** — footer `Next →` chains thread each track, and each track's last
   recipe carries the cross-track terminator.
5. **Rendering** — optionally serve locally (`python3 -m http.server`) and spot-check
   that the landing cards, track lists, badges, and highlighting render correctly.

> **Note:** the C++ listings cannot be compiled in this repo — there is no SDL3 /
> SDL_GPU / bgfx toolchain here. Listing correctness rests on matching the existing
> recipes' patterns and on doc-verified APIs, then compiling in the companion
> `sdl3-2d` template.
