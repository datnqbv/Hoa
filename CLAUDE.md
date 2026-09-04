# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a collection of standalone Vietnamese chemistry education simulation pages (`*.html`). Each file is a self-contained interactive lab simulation for high-school chemistry (the "Kết nối tri thức với cuộc sống" / "Hành trang số" textbook series). No build tools, package managers, or frameworks — open any file directly in a browser.

## Files

| File | Topic |
|------|-------|
| `aaa` / `abc.html` | Tính Bazơ Của Amine — Petri dish + pH bar, compares Methylamine / Ammonia / Water |
| `s1.html` | Tính Base Của Dung Dịch Amine — polished version comparing Methylamine / Ammonia / Aniline |
| `s2.html` | Amine + CuSO₄ — two-stage reaction (precipitate → dissolve into complex) using a test tube |
| `s3.html` | Amine + FeCl₃ — precipitate Fe(OH)₃, with control experiments |

## Architecture

Every simulation is a single-file HTML page following the same pattern:

- **HTML structure**: header → info panel → `<canvas>` → controls (buttons) → explanation box
- **State machine**: a handful of boolean flags (`isDropping`, `hasReacted`) and a `reactProgress` value (0→1) drive everything
- **Animation loop**: `requestAnimationFrame` calls a `loop()` function each frame; all drawing happens inside it
- **Color interpolation**: a `lerpColor(c1, c2, t)` utility animates color transitions smoothly
- **Drawing order per frame**: grid → static lab equipment → animated indicators/particles → pipette/dropper on top
- **Explanation panel**: `updateExplanation()` reads the current state and injects HTML into the explanation `<div>` below the canvas

### External dependencies (CDN, no local copies)
- `Plus Jakarta Sans` — Google Fonts
- `@tabler/icons-webfont` — jsDelivr (used in s1, s2, s3)
- Header background image — `www.aiducation.edu.vn` (used in s1, s2, s3)

## How to run / develop

Open any HTML file directly in a browser (no server needed):

```
# Windows — double-click, or from PowerShell:
Start-Process "e:\test\s1.html"
```

For live development, a simple static server also works:

```
npx serve e:\test
# or
python -m http.server 8080 --directory e:\test
```

## Key conventions

- All UI text is in **Vietnamese**.
- Canvas coordinates: `W × H` constants set at the top of each `<script>`; `CY` / `TY` is the vertical center of the lab equipment.
- Chemical labels in button text use Unicode subscripts (e.g. `CH₃NH₂`), but inside canvas `ctx.fillText` they often use plain ASCII (`CH3NH2`) because canvas font rendering is limited.
- Bubble/particle arrays are cleared on reset (`bubbles = []`, `particles = []`).
- The `aaa` file has no extension but is valid HTML — treat it the same as the `.html` files.
## AI Assistant Instructions

- Luôn trả lời bằng **tiếng Việt**