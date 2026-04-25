# AGENTS.md

## Project
Single-file Obsidian CSS theme snippet (`COLOURS.css`) stored in a vault's `.obsidian/snippets/` folder.

## Long-term Goal
This is intended to become a full Obsidian theme plugin in the future. For now, it is developed and iterated as a pure CSS snippet.

## Key Constraints
- **Pure CSS only.** No JavaScript. The snippet must remain distributable as a single `.css` file.
- Uses `oklch()` relative color syntax (`oklch(from var(--color-accent) ...)`). Requires a modern Obsidian version with up-to-date Chromium.

## File Overview
- `COLOURS.css` — The entire theme. Derives a full palette from the user's single accent color (`--color-accent`).
  - Light mode: fully implemented inside `body {}`
  - Dark mode: actively being converted from HSL to `oklch()`. `body.theme-dark` exists but is minimal.
  - Auto-contrast logic uses `--dynamic-switch` based on `round(calc(var(--contrast-threshold) - l))`.

## Test Tool
- `test/preview.html` — Standalone browser preview tool for rapid color testing without opening Obsidian.
  - Load `COLOURS.css` via file picker
  - Stress-test presets (`#B51A00`, `#FFFFFF`, `#000000`, `#808080`, `#FFD700`, `#00008B`)
  - Live color picker + Light/Dark mode toggle
  - Dynamic CSS variable inspector (parses `--*` from loaded CSS)
  - Copy computed values to clipboard
  - **Reload button** fetches `../COLOURS.css` (works with local servers; falls back to file picker on `file://`)

## Recently Added Styling
- **Blockquotes**: `.markdown-rendered blockquote` + `.cm-line.HyperMD-quote` (Live Preview)
- **Task checkboxes**: `.task-list-item-checkbox` + generic `input[type="checkbox"]`
- **Toggles**: `.checkbox-container` (removed `.modal` restriction for sidebar coverage). Uses `inset box-shadow` for borders to avoid thumb layout shift.
- **Sliders**: `input[type="range"]::-webkit-slider-thumb`
- **Tags**: Refactored to use CSS variables (`--tag-size`, `--tag-background`, `--tag-color`, etc.)

## Workflow
- No build, test, lint, or package tools. Edit `COLOURS.css` directly.
- To verify in Obsidian: place the file in `.obsidian/snippets/` and enable in **Settings → Appearance → CSS snippets**.
- Changes take effect immediately on save (Obsidian hot-reloads snippets).
- For faster iteration: open `test/preview.html` in a browser with a local server (`python3 -m http.server`) to use the Reload button.
