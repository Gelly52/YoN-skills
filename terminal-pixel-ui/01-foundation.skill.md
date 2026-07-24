# Foundation: Color Theory and Character Rendering

## The Terminal as a Pixel Canvas

A terminal emulator is a grid of character cells. Each cell has:
- Exactly one character (or two for CJK)
- A foreground color
- A background color
- Optional attributes (bold, dim, underline, reverse, italic)

This is equivalent to a very low-resolution display where each "pixel" is a character. The pixel-art aesthetic embraces this constraint rather than fighting it.

---

## Color Palette Design

### Palette Size

Retro systems used 4-16 simultaneous colors. For terminal pixel aesthetic, limit to **5-8 named semantic colors**. More than that creates visual noise.

### Palette Construction Method

1. Pick ONE dominant accent color (the "brand" tone)
2. Assign a color per severity/status level (3-5 levels)
3. Reserve white/gray for body text and metadata
4. Reserve one color for interactive/highlighted elements
5. Never assign two meanings to one color

### Semantic Color Roles

| Role | Description | Typical Choices |
|------|-------------|----------------|
| Accent | Frames, titles, branding | Gold, cyan, magenta, green |
| Critical/Error | Failures, urgent items | Red, bright_red |
| Warning | Caution, degraded state | Yellow, bright_yellow |
| Success/OK | Passing, healthy | Green, bright_green |
| Info/Neutral | Low-priority notices | Cyan, bright_blue |
| Muted | Metadata, hints, disabled | dim white, gray |
| Default | Body text | Terminal default (no override) |

### Terminal Color Compatibility

Color sources ranked by portability:

| Level | Support | Usage |
|-------|---------|-------|
| ANSI 8 | Universal | Critical fallbacks |
| ANSI 16 (bright variants) | Nearly universal | Primary palette |
| 256-color | Most modern terminals | Extended accents |
| True Color (24-bit) | Modern terminals only | Precise hex values |

Design the palette at ANSI-16 level first, then optionally enhance with 256/truecolor.

### Foreground vs Background

Pixel aesthetic strongly prefers **foreground-only coloring**:
- Text color carries the semantic meaning
- Background stays at terminal default (transparent to theme)
- Background color only used for: reverse-video badges, progress bar fills, and critical alerts
- This ensures the design works on both dark and light terminal themes

### Dim/Bold as Depth Layers

| Attribute | Visual Role |
|-----------|------------|
| `bold` (or `bright_*`) | Primary content, titles, active items |
| (normal) | Body text, data cells |
| `dim` | Metadata, timestamps, hints, decorative lines |

This creates 3 visual depth layers without additional colors.

---

## Character Rendering Palette

### Box-Drawing Characters (U+2500-U+257F)

The primary tool for creating "borders" in terminal pixel art:

| Weight | Corners | Horizontals | Verticals | Feel |
|--------|---------|-------------|-----------|------|
| Light | `┌ ┐ └ ┘` | `─` | `│` | Clean, technical |
| Rounded | `╭ ╮ ╰ ╯` | `─` | `│` | Softer, friendly |
| Heavy | `┏ ┓ ┗ ┛` | `━` | `┃` | Bold, urgent |
| Double | `╔ ╗ ╚ ╝` | `═` | `║` | Formal, classic |

Mixing weights creates visual hierarchy:
- Heavy for primary containers
- Light for nested/secondary content
- Double for special/hero sections

### Block Elements (U+2580-U+259F)

| Character | Name | Use |
|-----------|------|-----|
| `█` | Full block | Solid fills, progress bars, ASCII art shapes |
| `▓` | Dark shade | Partial fills, texture |
| `▒` | Medium shade | Lighter texture, disabled state |
| `░` | Light shade | Subtle watermarks, empty progress |
| `▀` | Upper half | Sub-cell vertical precision (top) |
| `▄` | Lower half | Sub-cell vertical precision (bottom) |
| `▌` | Left half | Sub-cell horizontal precision (left) |
| `▐` | Right half | Sub-cell horizontal precision (right) |

Block elements enable "double resolution" - using half-blocks effectively halves the pixel size.

### Common Indicators

| Category | Characters | Notes |
|----------|-----------|-------|
| Selection | `❯ › ▶ ▸ ►` | Active/pointed item |
| Checkmarks | `✓ ✔ ☑` vs `✗ ✘ ☒` | Pass/fail |
| Bullets | `● ○ ◉ ◎` | On/off, filled/empty |
| Arrows | `→ ← ↑ ↓ ⇒ ⇐` | Flow direction |
| Warnings | `⚠ ⚡ ☠ ⛔` | Attention markers |
| Stars | `★ ☆ ✦ ✧` | Rating, featured |
| Progress | `▏▎▍▌▋▊▉█` | 8-step horizontal bar |

---

## Typography Principles

### Size Hierarchy Without Font Sizes

Terminals cannot change font size. Simulate hierarchy with:

| Visual Size | Technique |
|-------------|----------|
| XXL | Figlet / ASCII art banner (5-8 lines tall) |
| XL | UPPER CASE + bold + accent color |
| L | Bold + accent color |
| M | Bold (default text level) |
| S | Normal weight |
| XS | Dim attribute |

### Alignment Grid

All content aligns to the character grid. Common alignment patterns:

- **Left-flush columns** in tables (default, easiest to scan)
- **Right-aligned numbers** (monetary, counts, percentages)
- **Center-aligned headers** in banners and splash screens
- **Indent-based nesting** (2 or 4 spaces per level, be consistent)

### CJK Mixed Content

CJK characters occupy 2 cells. When mixing:
- Use library-level cell-width calculation (wcwidth)
- Never rely on `len(string)` for alignment
- Test table alignment with mixed scripts
