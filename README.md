# COLOURS

A minimal, monospace, accent-tinted Obsidian theme that derives its entire palette from your single chosen accent color.

## Features

- **Dynamic palette** — Every color is derived from `--color-accent` using `oklch()` relative color syntax
- **Auto-contrast** — Text automatically switches between light and dark based on your accent color's lightness
- **Sharp edges** — Zero border radius everywhere for a crisp, technical feel
- **Monospace-first** — IBM Plex Mono preferred, with system monospace fallbacks
- **Full dark mode** — Complete dark palette with inverted backgrounds and fixed dark code blocks
- **Light & dark stress-tested** — Works with extreme accent colors (white, black, gold, pure red)

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
