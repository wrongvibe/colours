# AGENTS.md

## Project
Official Obsidian theme (`COLOURS`) stored in `.obsidian/themes/COLOURS/`.

## Key Constraints
- **Pure CSS only.** No JavaScript.
- Uses `oklch()` relative color syntax. Requires Obsidian 1.5.0+ (modern Chromium).
- **No remote assets.** All fonts are system/local only per [Theme guidelines](https://docs.obsidian.md/Themes/App+themes/Theme+guidelines).
- Follows official selector convention: `body` for shared, `.theme-light`/`.theme-dark` for palettes.

## File Overview
- `theme.css` — The entire theme stylesheet.
  - `body {}` — Shared structural variables (fonts, radius, heading sizes, tag padding, blockquote vars, Notebook Navigator vars).
  - `.theme-light {}` — Full light mode palette derived from `--color-accent`.
  - `.theme-dark {}` — Full dark mode palette with inverted backgrounds and fixed dark code blocks.
  - Component styles follow the mode blocks (shared across both modes).
- `manifest.json` — Theme metadata (name, version, author, minAppVersion).
- `versions.json` — Version-to-minAppVersion compatibility map.
- `package.json` + `version-bump.mjs` — Release automation (optional, for GitHub publishing).
- `COLOURS.css` — Archived original snippet (for reference).
- `test/preview.html` — Standalone browser preview tool.

## CSS Architecture
```css
body {          /* fonts, radius, structural vars */ }
.theme-light {  /* light palette */ }
.theme-dark {   /* dark palette */ }
/* component selectors follow */
```

## Test Tool
- `test/preview.html` — Standalone browser preview tool.
  - Load `theme.css` via file picker
  - Stress-test presets (`#B51A00`, `#FFFFFF`, `#000000`, `#808080`, `#FFD700`, `#00008B`)
  - Live color picker + Light/Dark mode toggle
  - Dynamic CSS variable inspector
  - Copy computed values to clipboard
  - **Reload button** fetches `../../themes/COLOURS/theme.css` (requires local server)

## Recently Added Styling
- **Full dark mode** — complete palette override in `.theme-dark`
- **Blockquotes**: `.markdown-rendered blockquote` + `.cm-line.HyperMD-quote` with custom border variables
- **Task checkboxes**: `.task-list-item-checkbox` + generic `input[type="checkbox"]`
- **Toggles**: `.checkbox-container` with inset `box-shadow` borders
- **Sliders**: `input[type="range"]::-webkit-slider-thumb`
- **Tags**: CSS variable-driven (`--tag-size`, `--tag-background`, `--tag-color`)
- **Unresolved links**: Fixed color and hover state
- **Empty state**: Themed `.empty-state` and `.empty-state-action`
- **Mobile toolbar**: Themed `.mobile-toolbar-option`
- **Canvas**: Node borders, edges (`canvas-display-path`), arrows, controls, card menu
- **is-flashing fix**: Override with `--text-highlight-bg` and `mix-blend-mode: normal`
- **Folder styles**: `.nav-folder-title-content` with default (light bg) and inverted preset
- **Cursor line**: `--cursor-line-color` variable for `.cm-active.cm-line`
- **Notebook Navigator**: Calendar theming (`--nn-theme-calendar-day-*`)
- **Graph view**: Node/link colours with HSL variables (`--graph-node`, `--graph-line`)
- **Bases embeds**: `--bases-embed-width` and `--bases-embed-transform` for controlling embedded Base width/position

### Bases Embed Width (`--bases-embed-width`)
Introduced in Obsidian 1.12.7 (2026 update). Controls width of embedded Bases (`![[Base.base]]`).

**Original Obsidian implementation** (extracted from `obsidian-1.12.7.asar`):
```css
/* Defined in .is-phone selector (line 17961) */
.is-phone {
  --bases-embed-width: calc(100% + var(--file-margins-x) * 2);
  --bases-embed-transform: translateX(calc(-1 * var(--file-margins-x)));
}

/* Applied in Reading View (line 13268) */
.markdown-reading-view .bases-embed {
  width: var(--bases-embed-width, 100%);  /* Fallback: 100% */
  transform: var(--bases-embed-transform, none);
}

/* Applied in Live Preview (line 13287) */
.markdown-source-view.mod-cm6 .cm-content > .bases-embed {
  width: var(--bases-embed-width, 100%);
  transform: var(--bases-embed-transform, none);
}
```

**Key insight**: The `width` property uses `var(--bases-embed-width, 100%)` with a fallback, meaning the variable doesn't need to be predefined in `body` — it only needs to be set where you want to override.

### Dynamic Transform Calculation (Avoid Hardcoding)
To make bases embeds ignore readable line length without hardcoding percentages:

```css
.markdown-source-view.mod-cm6 .cm-content > .bases-embed,
.markdown-source-view.mod-cm6 .cm-content > .cm-lang-base {
  --bases-embed-width: calc(var(--file-line-width) * 1.5);
  --bases-embed-transform: translateX(calc(
    (var(--file-line-width) - var(--bases-embed-width)) / 2 
    / var(--bases-embed-width) * 100%
  ));
}
```

**How it works**:
1. `(var(--file-line-width) - var(--bases-embed-width))` = Width difference (e.g., `700px - 1050px = -350px`)
2. `/ 2` = Half the difference = shift needed to center (`-175px`)
3. `/ var(--bases-embed-width)` = Convert to ratio of element's own width (`-175px / 1050px = -16.67%`)
4. `* 100%` = Convert to percentage for `translateX`

**Why this works**: `translateX(percentage)` uses the element's own width as reference, and all values come from CSS variables. Change the `1.5` multiplier and the transform adjusts automatically.

**Note**: The update package loads from `~/Library/Application Support/obsidian/obsidian-1.12.7.asar`, NOT the bundled `/Applications/Obsidian.app/Contents/Resources/obsidian.asar`.

## Workflow
- No build tools. Edit `theme.css` directly in `.obsidian/themes/COLOURS/`.
- To verify in Obsidian:
  1. **Settings → Appearance → Themes** → Select **COLOURS**
  2. Changes take effect immediately on save (Obsidian hot-reloads themes)
  3. Toggle **Base color scheme** between Light/Dark to test both palettes
- For faster iteration: open `test/preview.html` in a browser with a local server (`python3 -m http.server`) to use the Reload button.
- **Restart Obsidian** if you edit `manifest.json`.

## Reverse-Engineering Obsidian's Built-in CSS
Obsidian bundles its UI CSS inside `.asar` archives. To find the **real class names** (e.g. `.canvas-display-path` vs `.canvas-edge-path`), pseudo-elements, and default rules, extract and grep the official stylesheet.

**⚠️ Always use the LATEST package**: Obsidian auto-updates and loads from `~/Library/Application Support/obsidian/obsidian-*.asar`, NOT the bundled app. The bundled version may be outdated. Run Obsidian from terminal to see which package it loads.

### 1. Locate the asar files
**Important**: Obsidian loads from the **latest update package**, not the bundled version. Always check the update folder first.

```bash
# macOS - UPDATED PACKAGE (preferred, has latest code)
~/Library/Application Support/obsidian/obsidian-*.asar

# macOS - Bundled version (may be outdated)
/Applications/Obsidian.app/Contents/Resources/obsidian.asar
/Applications/Obsidian.app/Contents/Resources/app.asar

# Windows (typical)
%LOCALAPPDATA%\Obsidian\obsidian-*.asar
%LOCALAPPDATA%\Obsidian\app-*\resources\obsidian.asar

# Linux (typical)
~/.config/obsidian/obsidian-*.asar
/usr/lib/obsidian/resources/obsidian.asar
```

**Check which version Obsidian is using:**
```bash
# Run Obsidian from terminal to see loaded package
/Applications/Obsidian.app/Contents/MacOS/Obsidian

# Output will show: "Loading updated app package /Users/emma/Library/Application Support/obsidian/obsidian-1.12.7.asar"
```

### 2. Install asar extractor
```bash
npm install -g asar
# or
npx asar --version
```

### 3. Extract the UI CSS
```bash
mkdir -p /tmp/obsidian-ui
npx asar extract ~/Library/Application\ Support/obsidian/obsidian-*.asar /tmp/obsidian-ui
```
The main stylesheet is at `/tmp/obsidian-ui/app.css`.

**Note**: If the wildcard fails, check the exact filename:
```bash
ls ~/Library/Application\ Support/obsidian/*.asar
```

### 4. Search for selectors
```bash
# Example: find canvas edge classes
grep -n "canvas-display-path\|canvas-path-end\|canvas-interaction-path" /tmp/obsidian-ui/app.css

# Example: find mobile toolbar structure
grep -n "mobile-toolbar-option\|mobile-toolbar-options-list" /tmp/obsidian-ui/app.css

# Example: find pseudo-elements on navbar
grep -n -A3 "\.mobile-navbar::" /tmp/obsidian-ui/app.css
```

### 5. What to look for
- **Class names** — Obsidian often uses different names than community documentation (e.g. `.canvas-display-path` not `.canvas-edge-path`)
- **`!important` conflicts** — Obsidian sets inline SVG attributes; `!important` is often needed to override
- **Pseudo-elements** — `::before` and `::after` create borders, shadows, and gradients (e.g. the "white edge" on mobile toolbars)
- **`background-image` gradients** — These create subtle depth effects that look like borders
- **`var(--touch-background)`** — Mobile-specific background variable distinct from `--background-primary`
- **Mode-specific overrides** — `.is-mobile` vs `.is-phone` vs `.is-tablet`

### 6. Common gotchas
- For mobile: `.is-mobile`
- `box-shadow` on parent containers creates visible "edges" even when `border` is removed
- `backdrop-filter: blur()` creates frosted-glass effects that look like opacity/gradients
- The `app.asar` contains Electron app code; `obsidian.asar` contains the actual Obsidian UI CSS

## Migration Notes (from snippet)
- Moved from `.obsidian/snippets/COLOURS.css` → `.obsidian/themes/COLOURS/theme.css`
- Removed Google Fonts `@import`; font stack now prefers local `IBM Plex Mono` with system fallbacks
- Selector refactor: `body.theme-dark` → `.theme-dark` (matches Obsidian convention)
- Git repo moved from `.obsidian/snippets/` to `.obsidian/themes/COLOURS/`
