# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

COLOURS is a pure-CSS Obsidian theme. There is no build step — edit `theme.css` directly. No JavaScript.

## Development Workflow

**Test in Obsidian (hot-reload):**
1. Settings → Appearance → Themes → select **COLOURS**
2. Save `theme.css` — Obsidian reloads immediately
3. Toggle Light/Dark with **Base color scheme** to test both palettes

**Test in browser (faster iteration):**
```bash
python3 -m http.server   # from repo root
# then open test/preview.html in a browser
```
The Reload button in `preview.html` fetches `theme.css` via the local server. Use stress-test presets (`#B51A00`, `#FFFFFF`, `#000000`, `#808080`, `#FFD700`, `#00008B`) and the live color picker.

**Restart Obsidian** only when editing `manifest.json`.

## CSS Architecture

```css
/* @settings ... */   /* Style Settings plugin config — must stay at top */

body {}               /* fonts, radius, structural vars — shared both modes */
.theme-light {}       /* full light palette derived from --color-accent */
.theme-dark {}        /* full dark palette */
/* component selectors follow — shared across modes */
```

The entire palette is derived from `--color-accent` using `oklch()` relative color syntax (requires Obsidian 1.5.0+ / modern Chromium). Key derived relationships:

| Variable | Derivation |
|---|---|
| `--complement` | hue + 180° |
| `--triadic` | hue + 120° |
| `--split-comp` | hue + 150° |
| `--analogous` | hue + 30° |

`--dynamic-switch` reads accent lightness to auto-flip text between light and dark.

## Style Settings Integration

The `/* @settings ... */` block at the top of `theme.css` defines all user-configurable options. These drive `class-toggle` and `class-select` body classes. Any new setting must be added there, not just in CSS.

## Reverse-Engineering Obsidian CSS

The extracted Obsidian UI CSS is already available at `/Users/emma/vault118/__COLOURS/obsidian-ui/app.css`. To find real class names and defaults:

```bash
grep -n "selector-name" /Users/emma/vault118/__COLOURS/obsidian-ui/app.css
```

Key gotchas: use `!important` when overriding inline SVG attributes; check for `box-shadow` on parent containers creating visible edges; `.is-mobile` vs `.is-phone` vs `.is-tablet` for mobile selectors.

See `AGENTS.md` for the full extraction guide and common gotchas.

## Releasing

Bump version and tag — GitHub Actions publishes the release:

```bash
npm version patch   # bumps manifest.json + versions.json, stages them
git commit -m "chore: bump version"
git tag 0.0.2
git push && git push --tags
```

The workflow (`.github/workflows/release-version.yml`) uploads `theme.css` and `manifest.json` as release assets on any tag push.
