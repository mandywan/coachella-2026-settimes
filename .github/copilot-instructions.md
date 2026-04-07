# Copilot Instructions for coachella-2026-settimes

## Project shape
- This is a **single-file static web app**: all HTML, CSS, JS, and data live in [index.html](index.html).
- There is no framework, bundler, package manager, or backend.
- UI is optimized for a mobile viewport (`100dvh`, touch scrolling, compact typography).

## Core architecture
- `days` is the top-level schedule model: `const days = [fri, sat, sun]`.
- Each set entry uses tuple format:
  - `[stageIndex, artistName, startTime, endTime]`
  - Example: `[0, 'Anyma', '12:00', '12:50']`.
- Stage metadata is parallel arrays and index-aligned:
  - `stages` (labels)
  - `hdrBg` (header colors)
  - `clsMap` (set card class names)
- Rendering path is `showDay(i)` -> `build(days[i])`.

## Time and layout conventions (project-specific)
- `tm(t)` converts `h:mm` strings to minutes with a fixed `+12h` offset. This is a project-specific convention for festival timeline rendering.
- `f24(m)` formats minute values into `HH:MM` 24-hour labels.
- `build()` computes dynamic geometry from data + viewport:
  - Finds schedule range (`s0`/`e0`)
  - Computes pixels-per-minute from current scroll container height
  - Rebuilds headers, gutter labels, grid lines, and set blocks on each day switch/resize
- Horizontal sync pattern: `stages-scroll` controls the header strip transform on `onscroll`.

## Editing guidance
- Keep it dependency-free and inline unless explicitly asked otherwise.
- Preserve tuple schema for all day arrays; avoid introducing object-shaped records unless refactoring all read/write paths.
- If changing stage count/order, update **all** aligned structures (`stages`, `hdrBg`, `clsMap`, and data indices).
- Favor small DOM rebuild logic in `build()` over adding stateful component abstractions.
- Maintain compact class naming style used in this file (`.scol`, `.shdr`, `.tlbl`, `.set`, `.s0`-`.s7`).

## Local workflow
- Run locally as a static page; no build step is required.
- For reliable browser behavior, serve from workspace root with a lightweight static server.
- Main validation is manual:
  - Day tabs switch correctly
  - Horizontal scroll keeps headers aligned
  - Resize triggers correct relayout
  - Times/blocks render in expected order and height

## Key code anchors
- Data model and schedule arrays: [index.html](index.html)
- Rendering and layout engine: `build()` in [index.html](index.html)
- Day switching entry point: `showDay()` in [index.html](index.html)
- Boot/resize wiring: `window.addEventListener('load'|'resize', ...)` in [index.html](index.html)
