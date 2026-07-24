# Visual Component Patterns

These are **visual design patterns**, not implementation specifications. They describe what terminal UI elements should look like, not how to code them.

---

## Framed Container (Panel)

### Visual Structure

```
╭─ Title ──────────────────────────────────────────╮
│                                                   │
│  Content area with padding                        │
│                                                   │
╰──────────────────────────────────────────────────╯
```

### Design Rules

- Border: one consistent box-drawing weight (rounded, light, heavy, or double)
- Title: appears in top-left border, uses accent color + bold
- Optional subtitle: bottom-left border, uses dim/muted
- Internal padding: at least 1 char horizontal, 1 line vertical
- Border color: accent for primary, dim for secondary, severity color for alerts
- Max width: cap at 80-100 columns regardless of terminal width

### Hierarchy via Border Weight

| Priority | Border Style | Color |
|----------|-------------|-------|
| Hero/splash | Double `═║` or Heavy `━┃` | Accent |
| Primary content | Rounded `─│╭╮╰╯` | Accent |
| Secondary/nested | Light `─│┌┐└┘` | Dim |
| Alert/critical | Heavy `━┃┏┓┗┛` | Error color |

---

## Data Table

### Visual Structure

```
  Column A    Column B         Column C
  ──────────  ───────────────  ──────────────
  value       another value    description
  value       another value    description
```

Or with borders:

```
┃ Column A    ┃ Column B     ┃ Column C          ┃
┃ value       ┃ another      ┃ a longer text      ┃
┃ value       ┃ another      ┃ description here   ┃
```

### Design Rules

- Header row: bold or accent colored, visually distinct from data
- Column separator: either `┃` character or whitespace gap (2+ spaces)
- Row separator: optional; omit for dense output, include for readability
- Alignment: text left, numbers right, status center
- Truncation: ellipsis `…` for overflow, never wrap mid-word
- Color in cells: use semantic color only for the meaningful column (e.g., severity)

### Dense vs Readable

| Mode | Row height | Separators | When |
|------|-----------|-----------|------|
| Dense | 1 line | No row lines | Large result sets (>10 rows) |
| Readable | 1 line + gap | Row dividers | Small sets, mixed-width content |
| Card-per-row | 2-3 lines | Full box per item | Detail view, few items |

---

## Inline Badge / Tag

### Visual Patterns

```
 ERROR    (reverse-video red)
 WARN     (reverse-video yellow)
 OK       (reverse-video green)
```

Or without reverse:
```
[ERROR]  [WARN]  [OK]
```

### Design Rules

- Use reverse-video (swapped fg/bg) for maximum pop
- UPPERCASE for badge text
- Pad with one space on each side inside the badge
- Color maps directly to semantic meaning
- Maximum badge width: ~10 characters

---

## List Item / Row

### Visual Structure

```
  ● Item title                    metadata
    Description or secondary text
```

Or flat:
```
  ▶ active item (accent color)
    inactive item (default color)
    inactive item
```

### Design Rules

- Indicator character marks state (● filled = active, ○ empty = inactive)
- Title line: bold or accent, left-aligned
- Description: dim, indented below title or on same line after gap
- Metadata (time, count, status): right-aligned or pushed to end of line
- Selected item: accent color + indicator prefix (`❯`, `▶`, `›`)

---

## Progress Visualization

### Bar Style

```
Processing  [████████████░░░░░░░░]  62%
```

### Design Rules

- Filled portion: `█` in accent or success color
- Empty portion: `░` in dim/muted
- Bracket delimiters: `[` `]` in default color
- Label left, percentage right
- Fixed total width (20-40 chars for the bar itself)
- Alternative: use `▏▎▍▌▋▊▉█` for 8-step sub-character precision

### Spinner Style

For indeterminate operations, use a cycling character:
- Dots: `⠋⠙⠹⠸⠼⠴⠦⠧⠇⠏`
- Classic: `-\|/`
- Blocks: `▖▘▝▗`
- Always followed by a text description of what's happening

---

## Section Divider

### Visual Patterns

```
── Section Title ──────────────────────────────
```

Or:
```
═══════════════════════════════════════════════
```

Or with decorative elements:
```
──── ✦ Section Title ✦ ────────────────────────
```

### Design Rules

- Fill character: `─`, `━`, `═`, or `─` with decorative markers
- Title text centered or left-positioned within the line
- Color: dim for structural dividers, accent for section openers
- Never use plain dashes (`---`); always use proper box-drawing `─`

---

## Status Indicator Group

### Visual Pattern

```
● Service A        Running
● Service B        Running
○ Service C        Stopped
● Service D        Degraded
```

### Design Rules

- Dot color = status (green=healthy, red=error, yellow=degraded, gray=off)
- Name in default color
- Status text in matching color OR dim
- Consistent alignment across all rows
- Group under a section header or panel title

---

## Key-Value Display

### Visual Pattern

```
  Version:    1.4.2
  Status:     Active
  Last Run:   2 minutes ago
  Errors:     0
```

### Design Rules

- Keys: dim or muted, right-padded to consistent width
- Values: default weight or bold for important ones
- Colon + spacing between key and value (align all colons)
- Semantic color on values only when meaningful (red for errors > 0)
