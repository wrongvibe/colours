# COLOURS

A minimal, monospace, accent-tinted Obsidian theme that derives its entire palette from your single chosen accent color.

## Features

- **Dynamic palette** — Every color is derived from `--color-accent` using `oklch()` relative color syntax
- **Auto-contrast** — Text automatically switches between light and dark based on your accent color's lightness
- **Sharp edges** — Zero border radius everywhere for a crisp, technical feel
- **Monospace-first** — IBM Plex Mono preferred, with system monospace fallbacks
- **Full dark mode** — Complete dark palette with inverted backgrounds and fixed dark code blocks
- **Light & dark stress-tested** — Works with extreme accent colors (white, black, gold, pure red)
- **Style Settings support** — Customise highlight, heading, table, folder, uppercase, and heading font styles via the Style Settings plugin
- **Smart folder styling** — File explorer folders with optional inverted background treatment
- **Canvas support** — Themed node borders, edges, arrows, and controls
- **Notebook Navigator** — Calendar and list pane theming support
- **Respects official font settings** — Obsidian's built-in font picker overrides the theme when set

## Requirements

- Obsidian **1.5.0+** (required for `oklch(from ...)` relative color syntax)

## Installation

### From Obsidian Community Themes (when published)

1. Open **Settings → Appearance → Themes**
2. Click **Browse**
3. Search for **COLOURS**
4. Click **Install**

### Manual Installation

1. Download `theme.css` and `manifest.json`
2. Create a folder `.obsidian/themes/COLOURS/` in your vault
3. Place both files in that folder
4. Open **Settings → Appearance → Themes** and select **COLOURS**

## How It Works

The theme uses CSS `oklch()` relative color syntax to mathematically derive every hue, saturation, and lightness value from your chosen accent color:

- **Backgrounds** — Tinted with your accent hue, lightness varies by layer
- **Text** — Auto-contrasts using `--dynamic-switch` (reads accent lightness)
- **Headings** — Gradually fading opacity from H1 to H6
- **Code blocks** — Step function flips between light/dark based on accent
- **Highlights** — Complementary hue with guaranteed readable contrast

### Color Relationships

| Variable | Derivation |
|----------|-----------|
| `--complement` | Accent hue + 180° |
| `--triadic` | Accent hue + 120° |
| `--split-comp` | Accent hue + 150° |
| `--analogous` | Accent hue + 30° |

## Browser Preview Tool

A standalone preview tool is included for rapid testing without opening Obsidian:

1. Open `test/preview.html` in a modern browser
2. Load `theme.css` via the file picker
3. Try stress-test presets or pick any custom color
4. Toggle between Light and Dark modes
5. Inspect computed CSS variable values in real-time

For the **Reload** button to work, serve the directory with a local server:
```bash
python3 -m http.server
```

## Style Settings

Install the [**Style Settings**](https://github.com/mgmeyers/obsidian-style-settings) community plugin to customise these theme options. All settings are organised into collapsible groups:

### Typography

#### Heading Font
Set any custom font for headings using CSS `font-family` syntax.

- **Default:** `'Overused Grotesk Roman', 'Helvetica Neue', 'Inter', sans-serif`
- **Example:** `Georgia, serif` or `'Inter', sans-serif`

> Use quotes for font names containing spaces.

#### Disable Uppercase
Toggle off the theme's forced `text-transform: uppercase` on:
- Note titles (`.inline-title`)
- Buttons (modal, settings, ribbon, titlebar, view actions)
- Table headers (`th`)

#### Heading Style

Choose how markdown headings (`# H1` through `###### H6`) are coloured:

| Preset | Description |
|--------|-------------|
| **Monochrome** | Low-saturation accent tint — nearly grayscale |
| **A little bit colour** | Slightly more saturation |
| **Go Crazy** | Full chroma with different hues per heading level |

### Colours

#### Highlight Style

Choose how `==highlighted text==` appears:

| Preset | Description |
|--------|-------------|
| **Colourful** | Complementary hue with full saturation |
| **Subtle Colour** | Muted complementary tint |
| **Monochrome (High contrast)** | Dark text on light background (or vice versa) |
| **Monochrome (Low contrast)** | Subtle gray tones |

### Layout

#### Table Style

Choose table row styling:

| Preset | Description |
|--------|-------------|
| **Tinted header row** | Header row tinted with accent colour |
| **Tinted alternate rows** | Every other data row gets a subtle accent tint |

#### Folder Style

Choose how folder names appear in the file explorer:

| Preset | Description |
|--------|-------------|
| **Light Background** | Subtle tinted background behind folder names |
| **Inverted Background** | High-contrast muted background with primary text colour |

## Fonts

The theme respects Obsidian's official font settings:

- **Interface font** — Obsidian Settings → Appearance → Interface font overrides the theme's IBM Plex Mono
- **Text font** — Obsidian Settings → Appearance → Text font overrides the theme's IBM Plex Mono
- **Monospace font** — Obsidian Settings → Appearance → Monospace font overrides the theme's IBM Plex Mono
- **Heading font** — Controlled via Style Settings (Typography → Heading Font)

If you leave Obsidian's font settings empty, the theme falls back to its monospace stack.

## File Structure

```
.obsidian/themes/COLOURS/
├── theme.css              # Main stylesheet
├── manifest.json          # Theme metadata
├── README.md              # This file
├── test/
│   └── preview.html       # Browser preview tool
└── versions.json          # Version compatibility map
```

## Customisation

All variables are CSS custom properties — override them in a snippet if needed:

```css
.theme-light, .theme-dark {
  --tag-size: 1em;
  --code-bg-threshold: 0.8;
}
```

## Contributing

This theme is pure CSS. Edit `theme.css` directly — no build step required.

## License

[Your license here]