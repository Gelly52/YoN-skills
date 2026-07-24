# Structure: Hierarchy, Rhythm, and Information Density

How to organize terminal output so it reads clearly within the pixel-art aesthetic.

---

## Visual Weight Hierarchy

Terminal output has no z-index or layering. Hierarchy is expressed through:

| Weight | Technique | Usage |
|--------|-----------|-------|
| Heaviest | ASCII art + colored panel + heavy borders | Startup banner (once) |
| High | Accent-colored panel with title | Primary results, reports |
| Medium | Section header (`== TITLE ==` or rule) | Group separator |
| Normal | Plain text, optionally bold | Body content |
| Low | Dim text, indented | Metadata, timestamps, hints |
| Lightest | Single dim line | Footers, summaries |

### Golden Rule

If everything is highlighted, nothing is highlighted. Use heavy visual weight sparingly:
- ONE splash banner per session
- Panels for important results only (not every output)
- Accent color for titles and key values, not entire paragraphs

---

## Information Density

### Compact Mode (High Density)

For result lists, logs, or scan output:

```
ERR  config.py:12     Missing required field
ERR  handler.py:34    Unreachable code after return
WARN utils.py:8       Unused import
INFO main.py:1        Module docstring missing
```

Rules:
- No borders, no panels
- Fixed-width columns aligned by character position
- Color only on the severity column
- One item per line
- Summary count at bottom

### Standard Mode (Medium Density)

For structured reports:

```
╭─ Report Title ─────────────────────────────╮
│                                             │
│  Header    Value        Detail              │
│  ────────  ──────────   ─────────────────   │
│  item      value        description here    │
│  item      value        description here    │
│                                             │
│  Summary: 2 items found                     │
│                                             │
╰─────────────────────────────────────────────╯
```

Rules:
- Panel wraps the entire result
- Table inside with headers
- Summary line at bottom of panel
- Breathing room (empty lines above/below content)

### Expanded Mode (Low Density)

For single-item detail views:

```
╭─ Item Detail ──────────────────────────────╮
│                                             │
│  Name:        Widget Factory                │
│  Status:      Active                        │
│  Created:     2024-01-15                    │
│                                             │
│  Description:                               │
│  A longer text that wraps within the        │
│  panel boundaries at word breaks.           │
│                                             │
╰─────────────────────────────────────────────╯
```

Rules:
- Key-value pairs with aligned colons
- Multi-line text wraps inside panel
- Generous padding
- One panel per item

---

## Visual Rhythm

### The Pattern: Header → Body → Footer

Every output block follows:

1. **Header**: what this output IS (command name, section title)
2. **Body**: the actual data or content
3. **Footer**: summary, count, or next-action hint

### Spacing Cadence

| Between... | Spacing |
|-----------|--------|
| Panel and next panel | 1 blank line |
| Section header and content | 0 lines (header is the opener) |
| Table rows | 0 lines (dense) or 1 line (readable) |
| Unrelated output blocks | 1-2 blank lines |
| Last output and prompt | 1 blank line |

Consistency matters more than any specific value. Pick one spacing scheme and maintain it.

### Repetition Creates Pattern

When showing multiple items of the same type:
- Identical structure for each item
- Identical indentation
- Identical color scheme
- The human eye recognizes the pattern and can scan faster

---

## Width Management

### The 80/100 Rule

- Design for **80 columns** minimum (SSH sessions, split terminals)
- Cap panels at **100 columns** maximum (ultra-wide terminals make long lines unreadable)
- Tables can use full terminal width (they benefit from space)

### Width Calculation

```
Effective width = min(terminal_width - 2, MAX_CAP)
```

The `-2` accounts for potential left margin or scrollbar.

### Narrow Graceful Degradation

| Terminal Width | Adaptation |
|--------------|------------|
| >= 100 | Full design, panels capped at 100 |
| 80-99 | Full design, panels fill available space |
| 60-79 | Drop decorative elements, simplify panels |
| < 60 | Plain text mode, no panels |

---

## Nesting and Depth

### Indentation

Express parent-child relationships with consistent indent:

```
Root Item
  Child Item
    Grandchild Item
      Leaf Item
```

Rules:
- 2 spaces per level (compact, suitable for deep trees)
- OR 4 spaces per level (readable, maximum 3 levels deep)
- Never mix indent widths

### Tree Lines

For explicit tree structures:

```
├── First child
│   ├── Nested child
│   └── Last nested
├── Second child
└── Last child
```

### Flow Arrows

For sequential/causal chains:

```
Input
  → Process A
    → Process B
      → Output (result)
```

Arrow character (`→`, `⇒`, `▸`) indicates direction/causality.

---

## Practical Composition Rules

### Rule 1: One Hero Per Screen

Only one element should use maximum visual weight at any time. If you have a splash banner displayed, don't also have a heavy-bordered alert.

### Rule 2: Color Carries Meaning

If a piece of text is colored, that color must mean something. If it's just decoration, use the accent color. Never use random colors.

### Rule 3: Borders Cost Attention

Every bordered panel claims visual priority. Ask: does this content NEED to be framed? If it's part of a flowing conversation or repeated output, plain text (possibly indented) is better.

### Rule 4: Align Everything

In a monospace grid, misalignment is immediately visible and looks broken. Numbers align right, text aligns left, status indicators align center. Test with varying content lengths.

### Rule 5: Dim is Your Best Friend

The `dim` attribute is the most powerful tool for establishing hierarchy. Metadata, timestamps, hints, decorative borders - anything that supports but doesn't need attention should be dim.

### Rule 6: Summary Closes the Loop

Every substantive output block ends with a one-line summary. The user should never have to count items manually or scroll back to understand the scope.

```
...item list...

Total: 12 items (3 errors, 9 ok)
```
