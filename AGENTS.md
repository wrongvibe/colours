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
  - Dark mode: incomplete. `body.theme-dark` exists but most rules are commented out or placeholders.
  - Auto-contrast logic uses `--dynamic-switch` based on `round(calc(var(--contrast-threshold) - l))`.

## Workflow
- No build, test, lint, or package tools. Edit `COLOURS.css` directly.
- To verify: place the file in any Obsidian vault's `.obsidian/snippets/` folder and enable it in **Settings → Appearance → CSS snippets**.
- Changes take effect immediately on save (Obsidian hot-reloads snippets).
