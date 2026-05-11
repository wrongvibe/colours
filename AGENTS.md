# AGENTS.md

## Project
Official Obsidian theme (`COLOURS`) stored in `.obsidian/themes/COLOURS/`.

## Key Constraints
- **Pure CSS only.** No JavaScript.
- Uses `oklch()` relative color syntax. Requires Obsidian 1.5.0+ (modern Chromium).
- **No remote assets.** All fonts are system/local only per [Theme guidelines](https://docs.obsidian.md/Themes/App+themes/Theme+guidelines).
- Follows official selector convention: `body` for shared, `.theme-light`/`.theme-dark` for palettes.
- **Zero border radius throughout** — `--radius-s/m/l/xl` and all component radius vars set to `0px` in `body {}`.

## File Overview
- `theme.css` — The entire theme stylesheet (see CSS Architecture below).
- `manifest.json` — Theme metadata (name, version, author, minAppVersion).
- `versions.json` — Version-to-minAppVersion compatibility map.
- `package.json` + `version-bump.mjs` — Release automation (optional, for GitHub publishing).
- `archive/COLOURS.css` — Original snippet (historical reference only).
- `test/preview.html` — Standalone browser preview tool.

## CSS Architecture

```
/* @settings ... */          Style Settings YAML block (must stay at top of file)

body {}                      Shared: fonts, radius, hue-shift vars, structural vars
                             Hue-shift vars: --complement, --triadic, --triadic-2,
                             --analogous, --analogous-2/3/4, --split-comp

/* Style Settings presets */ heading-colour/colourful, highlight-subtle/monochrome-*,
                             heading-size-compact/large, line-height-relaxed/compact

.theme-light {}              Full light palette (--color-accent derived via oklch)
.theme-dark {}               Full dark palette (inverted backgrounds, fixed code blocks)

/* Accent override */        .theme-light.accent-override, .theme-dark.accent-override

/* Component styles */       All component selectors (shared across both modes):
                             status bar, scrollbar, dividers, flashing fix
                             folder styles, outline headings, inline title, view headers
                             task checkboxes (standard + alternate), blockquotes
                             sliders, buttons, tags, selected text, code blocks
                             links, tables, empty state, Vim cursor
                             mobile, canvas, callouts, mermaid, Notebook Navigator
                             Style Settings overrides (disable-uppercase, folder-plain, etc.)
```

## Style Settings Structure

The `/* @settings ... */` YAML block drives the Style Settings plugin. Current sections and settings:

**Colours** (heading level 1)
- `accent-override` — class-toggle → `.accent-override` on body
- `accent-colour` — variable-themed-color → `--accent-colour` CSS var
- `heading-preset` — class-select → `.heading-monochrome` (default, no CSS) / `.heading-colour` / `.heading-colourful`
- `highlight-preset` — class-select → `.highlight-default` (default) / `.highlight-subtle` / `.highlight-monochrome-high` / `.highlight-monochrome-low`

**Typography** (heading level 2)
- `font-heading-override` — variable-text → `--font-heading-override` CSS var
- `heading-size-preset` — class-select → `.heading-size-default` (no CSS) / `.heading-size-compact` / `.heading-size-large`
- `line-height-style` — class-select → `.line-height-default` (no CSS) / `.line-height-relaxed` / `.line-height-compact`
- `disable-uppercase` — class-toggle → `.disable-uppercase` on body

**Layout** (heading level 2)
- `table-style` — class-select → `.table-default` (no CSS) / `.table-alternate-rows`
- `folder-style` — class-select → `.folder-default` (no CSS) / `.folder-inverted` / `.folder-plain`
- `internal-link-style` — class-select → `.link-wavy` (default) / `.link-underline` / `.link-none`
- `auto-hide-scrollbar` — class-toggle → `.auto-hide-scrollbar` on body

**Bases** (heading level 2)
- `bases-embed-width-override` — variable-number-slider (1–5, default 1.5) → `--bases-embed-width-override` CSS var

**Rule for default values**: The default option in a class-select never needs its own CSS class — the base component styles already implement the default appearance. Only non-default options get CSS rules.

## Alternate Checkbox System

All alternate checkboxes (`[/]`, `[>]`, `[<]`, `[?]`, `[-]`, `[!]`, `[i]`, `[*]`, `[b]`, `[l]`, `[k]`, `[u]`, `[d]`, `[p]`, `[c]`) use a shared `::after` pseudo-element with `-webkit-mask-image` for the icon.

**Two groups with different hover behaviour:**

| Group | Types | At rest | On hover |
|-------|-------|---------|----------|
| **Icon-only** | `*`, `b`, `l`, `k` | `background: transparent`, `border: none` | bg stays transparent, icon → `--text-normal` |
| **Box-bg** | all others | coloured background, varies | `background-color` changes via base hover rule, `box-shadow: inset 0 0 0 1px` ring, icon → `--text-normal` |

**Group 2 specifics** (`[?]`, `[-]`, `[!]`, `[i]`, `[*]`, `[b]`, `[l]`, `[k]`, `[u]`, `[d]`, `[p]`, `[c]`):
- Base group sets `border: none` at rest
- Hover overrides must explicitly set `border: none` to block the base `.task-list-item-checkbox:checked:hover` rule, which would otherwise add a `1px solid` border and shift the `::after` (since `inset: 0` positions relative to the padding box)
- Use `box-shadow: inset 0 0 0 1px` instead for the visible ring — no box-model impact

**Group 1 specifics** (`[/]`, `[>]`, `[<]`):
- These have an Obsidian-default border at rest, so the base hover rule's border-color change is fine — do NOT override `border: none` for these

**Selector pattern**: Each alternate checkbox requires 3 selector variants:
```css
input[data-task="X"]:checked { }
li[data-task="X"] > input:checked { }
li[data-task="X"] > p > input:checked { }
```

## Bases Embed Width

Controlled via the `--bases-embed-width-override` CSS variable (set by Style Settings slider, default `1.5`).

The implementation in `theme.css` calculates embed width and centering transform dynamically:
```css
.markdown-source-view.mod-cm6.is-readable-line-width .cm-content > .bases-embed {
  --bases-embed-width: calc(var(--file-line-width) * var(--bases-embed-width-override));
  --bases-embed-transform: translateX(calc(
    (var(--file-line-width) - var(--bases-embed-width)) / 2
    / var(--bases-embed-width) * 100%
  ));
}
```
`translateX(percentage)` uses the element's own width as reference — all values stay relative, no hardcoded pixels.

## Obsidian Built-in CSS

The extracted Obsidian UI CSS is already available — no asar extraction needed:

```bash
grep -n "selector-name" /Users/emma/vault118/__COLOURS/obsidian-ui/app.css
```

**Main stylesheet:** `/Users/emma/vault118/__COLOURS/obsidian-ui/app.css`

Use this to:
- Find real class names (Obsidian often differs from community docs, e.g. `.canvas-display-path` not `.canvas-edge-path`)
- Check default variable values before overriding (avoid redundant declarations)
- Find pseudo-elements, `!important` conflicts, and mode-specific selectors (`.is-mobile` / `.is-phone` / `.is-tablet`)

Common gotchas when overriding:
- `box-shadow` on parent containers creates visible "edges" even when `border` is removed
- Obsidian sets inline SVG attributes — often needs `!important` to override
- `backdrop-filter: blur()` creates frosted-glass effects
- `app.asar` = Electron app code; `obsidian.asar` = the actual UI CSS

## Workflow
- No build tools. Edit `theme.css` directly.
- To verify in Obsidian:
  1. **Settings → Appearance → Themes** → Select **COLOURS**
  2. Changes take effect immediately on save (Obsidian hot-reloads themes)
  3. Toggle **Base color scheme** between Light/Dark to test both palettes
- For faster iteration: open `test/preview.html` with `python3 -m http.server` to use the Reload button.
- **Restart Obsidian** if you edit `manifest.json`.

## Test Tool (`test/preview.html`)
- Load `theme.css` via file picker (or Reload button with local server)
- Stress-test presets: `#B51A00`, `#FFFFFF`, `#000000`, `#808080`, `#FFD700`, `#00008B`
- Live color picker + Light/Dark toggle
- Dynamic CSS variable inspector + copy to clipboard
