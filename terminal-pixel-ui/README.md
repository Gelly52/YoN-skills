# Terminal Pixel Aesthetic

A design methodology for achieving retro/pixel-art visual style in terminal applications. This skill teaches principles and techniques - not a specific architecture or library binding.

## What This Skill Is

A set of visual design principles for making terminal output look like retro game interfaces: high contrast, hard borders, limited palette, blocky characters, and discrete animations.

## What This Skill Is NOT

- Not a framework or library recommendation
- Not a project template or file structure guide
- Not tied to any specific language or toolkit
- Not an interaction design guide (no REPL patterns, no menu logic)

## Skill Files

| File | Focus |
|------|-------|
| `01-foundation.skill.md` | Color theory, character rendering palette, typography principles |
| `02-components.skill.md` | Visual patterns for containers, data displays, and indicators |
| `03-effects.skill.md` | ASCII art techniques, motion simulation, decorative elements |
| `04-structure.skill.md` | Information density, visual rhythm, hierarchy expression |

## Core Idea

Terminals are inherently pixel grids. Each character cell is a fixed-size tile. This design system treats the terminal as a low-resolution display and applies 8-bit era visual principles:

1. **Limited Palette** - 5-8 colors maximum, each with a clear semantic role
2. **Hard Borders** - Unicode box-drawing characters, never soft/rounded CSS-style
3. **Block Fills** - Full/half/quarter block characters for sub-cell precision
4. **Flat Color** - No gradients, no blur, no transparency simulation
5. **Monospace Alignment** - Exploit the grid; align everything to character boundaries
6. **Width Discipline** - Never stretch beyond comfortable reading width (~80-100 cols)
