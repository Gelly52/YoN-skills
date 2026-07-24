# Effects: ASCII Art, Motion, and Decoration

Techniques for adding visual personality and motion to terminal output while maintaining the pixel-art aesthetic.

---

## ASCII Art Banner Design

### Figlet Text

Large text banners for application titles, splash screens, or section openers. Created from monospace "font" patterns that span 5-8 lines vertically.

```
 _____ _ _   _
|_   _(_) |_| | ___
  | | | | __| |/ _ \
  | | | | |_| |  __/
  |_| |_|\__|_|\___|
```

### Guidelines

1. **Maximum height**: 6-8 lines (anything taller pushes useful content off screen)
2. **Maximum width**: must fit within the narrowest supported terminal (80 cols)
3. **Font choice**: "standard", "slant", "small" are safest; avoid overly decorative fonts
4. **Color**: accent color + bold; single color only (no rainbow)
5. **Placement**: centered horizontally within a panel or screen
6. **Usage**: splash screen only; never use figlet in repeated output

### Block-Element Art

Drawing shapes and icons using `█▓▒░▀▄▌▐` characters:

```
  ███████
  █     █
  █ ░░░ █
  █ ░░░ █
  █     █
  ███████
```

### Guidelines

1. Keep icons small: 8-15 chars wide, 6-12 lines tall
2. Use box-drawing for outer frame, blocks for interior fill
3. One accent color for the entire icon
4. Icons appear only in splash/banner contexts
5. Pair icon with figlet text in a side-by-side grid layout
6. Test rendering in multiple terminal fonts (some render blocks with gaps)

---

## Motion and Animation

### What's Possible in a Terminal

| Technique | How | When |
|-----------|-----|------|
| Spinner | Cycle characters in-place (CR overwrite) | Indeterminate waits |
| Progress bar | Redraw a single line with growing fill | Determinate operations |
| Live-updating table | Redraw entire region (cursor control) | Streaming results |
| Typewriter text | Print characters one-by-one with delay | Dramatic reveals |
| Fade-in | Print dim → normal (redraw line) | Section entry |

### What to Avoid

- Full-screen redraws for simple status updates
- Animations that block user input
- Effects longer than 2 seconds for non-interactive output
- Multiple simultaneous animations competing for attention

### Typewriter Principle

Print text character-by-character with a micro-delay (10-30ms per char):
- Use only for agent/AI responses or important reveals
- Allow interruption (any keypress skips to full display)
- Never apply to error messages or actionable output

### Live Update Principle

For output that changes over time (streaming results, progress):
- Designate a fixed screen region
- Redraw only that region
- Keep the rest of the terminal history intact
- Always show final static result when done (don't leave only the animated version)

---

## Decorative Elements

### Horizontal Rules

Separators between logical sections:

| Style | Characters | Mood |
|-------|-----------|------|
| Light | `────────────────` | Subtle separation |
| Heavy | `━━━━━━━━━━━━━━━━` | Strong break |
| Double | `════════════════` | Formal division |
| Dashed | `╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌` | Light, technical |
| Dotted | `┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄┄` | Very subtle |
| Decorated | `──── ✦ text ✦ ────` | Section with label |

### Corner Decorations

Small decorative marks at content boundaries:
```
◆─────────────────────────◆
    content here
◆─────────────────────────◆
```

### Symbolic Grouping

Use Unicode symbols as category markers:
```
  ◈ Category One
  ◈ Category Two
  ◈ Category Three
```

Or with alignment:
```
  [1] First item
  [2] Second item
  [3] Third item
```

---

## Color as Effect

### Severity Gradient

When displaying mixed-priority items, color alone creates visual "heat mapping":

```
red      CRITICAL  item text...
red      ERROR     item text...
yellow   WARNING   item text...
cyan     INFO      item text...
dim      DEBUG     item text...
```

The eye naturally gravitates to warm/bright colors first.

### Reverse-Video Flash

Draw maximum attention to a single word or phrase:
```
Status: [reverse red] FAILED [/reverse]
```

Use sparingly - maximum 1-2 reverse-video elements per screen.

### Dim-to-Bright Transition

Show state change by contrasting old (dim) with new (bold):
```
Count: 12 → 15 (+3)
       dim  bold  accent
```

---

## Anti-Patterns

| Don't | Why |
|-------|-----|
| Rainbow text (many colors per line) | Breaks pixel aesthetic; looks chaotic |
| Background color blocks as layout | Breaks on light terminals; looks dated |
| Blinking text (ANSI blink) | Universally annoying; rarely supported |
| ASCII art in every output | Only in splash/help; data output must be clean |
| Decorative borders on ephemeral output | Adds visual noise to high-frequency content |
| Full-width stretching | Reading lines > 100 chars is painful |
