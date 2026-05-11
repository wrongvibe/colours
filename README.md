# COLOURS

A minimal, monospace, accent-tinted Obsidian theme. The entire colour palette — backgrounds, text, headings, highlights, borders — is derived mathematically from your single chosen accent colour using CSS `oklch()` relative colour syntax.

## Features

- **Single-accent palette** — Every colour derived from `--color-accent` via `oklch()`; change one colour, everything updates
- **Auto-contrast** — Text automatically flips light/dark based on your accent's lightness
- **Sharp edges** — Zero border radius throughout for a crisp, technical feel
- **Monospace-first** — IBM Plex Mono preferred, with system monospace fallbacks
- **Full dark mode** — Complete palette override with inverted backgrounds and fixed dark code blocks
- **Stress-tested** — Works with extreme accent colours: white, black, gold, pure red
- **Style Settings** — Typography, colour, layout, and Bases options via the Style Settings plugin
- **Custom task checkboxes** — Styled alternates for `[*]`, `[!]`, `[?]`, `[i]`, `[>]`, `[<]`, `[-]`, `[b]`, `[l]`, `[k]`, `[u]`, `[d]`, `[p]`, `[c]`
- **Canvas** — Themed node borders, edges, arrows, and controls
- **Notebook Navigator** — Calendar and list pane theming
- **Respects Obsidian font settings** — Interface/Text/Monospace font pickers override the theme when set

## Requirements

Obsidian **1.5.0+** (required for `oklch(from …)` relative colour syntax — ships with a modern Chromium base).

## Installation

### Community Themes (when published)

1. **Settings → Appearance → Themes → Browse**
2. Search **COLOURS** → Install

### Manual

1. Download `theme.css` and `manifest.json`
2. Place both in `.obsidian/themes/COLOURS/` inside your vault
3. **Settings → Appearance → Themes** → select **COLOURS**

## Style Settings

Install the [**Style Settings**](https://github.com/mgmeyers/obsidian-style-settings) community plugin to access these options.

---

### Colours

#### Override System Accent
When off (default), Obsidian's built-in accent colour picker drives the theme.
Turn **on** to use the per-mode colours below instead of the system accent.

#### Accent Colour
*(Only active when Override System Accent is on.)*
Set separate accent colours for light and dark mode. Defaults: `#0d0d73` (light) / `#0f0f3d` (dark).

#### Heading Style
How `# H1` through `###### H6` are coloured:

| Option | Description |
|--------|-------------|
| **Monochrome** *(default)* | Low-saturation accent tint — nearly grayscale |
| **A little bit colour** | Slightly more chroma |
| **Go Crazy** | Full chroma, different hue per heading level |

#### Highlight Style
How `==highlighted text==` appears:

| Option | Description |
|--------|-------------|
| **Colourful** *(default)* | Complementary hue, full saturation |
| **Subtle Colour** | Muted complementary tint |
| **Monochrome (High contrast)** | Dark on light (or vice versa) |
| **Monochrome (Low contrast)** | Subtle grey tones |

---

### Typography

#### Heading Font
Set any font for headings using CSS `font-family` syntax.

- **Default:** `'Overused Grotesk Roman', 'Helvetica Neue', 'Inter', sans-serif`
- **Example:** `Georgia, serif` or `'Departure Mono', monospace`

Quote names that contain spaces.

#### Heading Sizes
Scale all heading sizes up or down:

| Option | H1 → H6 range |
|--------|---------------|
| **Default** | 2.5em → 1.1em |
| **Compact** | 1.8em → 1.0em |
| **Large** | 3.0em → 1.15em |

#### Line Height

| Option | Value |
|--------|-------|
| **Default** | Obsidian default |
| **Relaxed** | 1.8 |
| **Compact** | 1.4 |

#### Disable Uppercase
Toggles off the theme's `text-transform: uppercase` on note titles, buttons, and table headers.

---

### Layout

#### Table Style

| Option | Description |
|--------|-------------|
| **Tinted header row** *(default)* | Header row tinted with accent |
| **Tinted alternate rows** | Zebra stripe on data rows |

#### Folder Style

| Option | Description |
|--------|-------------|
| **Light Background** *(default)* | Subtle accent tint behind folder names |
| **Inverted Background** | High-contrast muted background with primary text colour |
| **Plain** | No background — unstyled folder names |

#### Internal Link Decoration

| Option | Description |
|--------|-------------|
| **Wavy** *(default)* | Wavy underline on internal links |
| **Underline** | Solid underline |
| **None** | No decoration |

#### Auto-hide Scrollbar
Uses the OS overlay scrollbar — appears on hover/scroll, hidden otherwise. Replaces the theme's custom thin scrollbar.

---

### Bases

#### Base Embed Width Multiplier
Controls how wide embedded Bases (`![[Base.base]]`) are relative to the readable line width.
- **Range:** 1× (same as line width) → 5× wider
- **Default:** 1.5×

Only has a visible effect when readable line length is enabled.

---

## Fonts

The theme respects Obsidian's official font settings:

| Obsidian setting | Overrides |
|-----------------|-----------|
| Interface font | Theme's IBM Plex Mono interface stack |
| Text font | Theme's IBM Plex Mono text stack |
| Monospace font | Theme's IBM Plex Mono monospace stack |
| *(leave empty)* | Theme's font stack applies |

Heading font is controlled separately via Style Settings → Typography → Heading Font.

## Browser Preview Tool

A standalone preview is included for rapid testing without Obsidian:

```bash
python3 -m http.server   # serve from repo root
# then open test/preview.html
```

- Load `theme.css` via the file picker (or use the Reload button with the local server)
- Try stress-test presets: `#B51A00`, `#FFFFFF`, `#000000`, `#808080`, `#FFD700`, `#00008B`
- Toggle Light / Dark mode
- Inspect computed CSS variable values live

## Customisation

All colours are CSS custom properties. Override any variable in a snippet:

```css
.theme-light, .theme-dark {
  --tag-size: 1em;
}
```

## Contributing

Pure CSS — no build step. Edit `theme.css` directly.

## License

MIT
