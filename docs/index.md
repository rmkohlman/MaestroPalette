# MaestroPalette

MaestroPalette is a Go package (`github.com/rmkohlman/MaestroPalette`) that provides shared color palette primitives for the DevOpsMaestro ecosystem. It is used by MaestroNvim, MaestroTerminal, dvm, and related tools to ensure consistent color handling across all consumers.

Package name: `palette`

Zero external Go dependencies. Pure stdlib.

## What It Provides

| Area | Description |
|------|-------------|
| `Palette` struct | Core type with semantic color map, metadata fields, and methods |
| Color key constants | 55 semantic string constants for backgrounds, foregrounds, UI, diagnostics, accents, and ANSI terminal slots |
| Hex utilities | Validation, normalization, RGB parsing, ANSI 256-color conversion |
| HSL color space | Struct and functions for converting, lightening, darkening, rotating, saturating, and desaturating colors |
| Terminal extraction | Mapping semantic palette colors to the ANSI 16-color set with fallback chains |

## Installation

```sh
go get github.com/rmkohlman/MaestroPalette
```

## Quick Start

### Build a palette and read colors

```go
import "github.com/rmkohlman/MaestroPalette"

p := &palette.Palette{
    Name:     "my-theme",
    Category: palette.CategoryDark,
    Colors: map[string]string{
        palette.ColorBg:    "#1a1b26",
        palette.ColorFg:    "#c0caf5",
        palette.ColorError: "#f7768e",
        palette.ColorInfo:  "#7aa2f7",
    },
}

if err := p.Validate(); err != nil {
    // handle validation error
}

bg := p.GetOrDefault(palette.ColorBg, "#000000") // "#1a1b26"
```

### Merge and clone

```go
base := &palette.Palette{
    Name: "base",
    Colors: map[string]string{
        palette.ColorBg: "#000000",
        palette.ColorFg: "#ffffff",
    },
}

overlay := &palette.Palette{
    Name: "overlay",
    Colors: map[string]string{
        palette.ColorBg:    "#1a1b26",
        palette.ColorError: "#ff0000",
    },
}

// Merge overlay into a clone of base; do not overwrite existing keys
merged := base.Clone()
merged.Merge(overlay, false)
// merged.Get(palette.ColorBg) == "#000000"  (not overwritten)
// merged.Get(palette.ColorError) == "#ff0000" (added)
```

### HSL color manipulation

```go
hsl, err := palette.HexToHSL("#7aa2f7")
if err != nil {
    // handle error
}

// Lighten by 10 percentage points of lightness
lighter := hsl.Lighten(0.1)

// Rotate hue by 30 degrees
rotated := hsl.Rotate(30)

// Convert back to hex
hex := rotated.ToHex() // e.g. "#rrggbb"
```

### Extract terminal colors

```go
// Map semantic palette colors to the ANSI 16-color terminal set
termColors := p.ToTerminalColors()
// termColors[palette.TermRed], termColors[palette.TermBlue], etc.

// Or produce a new Palette containing only the terminal-relevant colors
termPalette := p.TerminalPalette()
// termPalette.Name == "my-theme-terminal"
```

## Further Reading

- [Palette](palette.md) -- `Palette` struct, color key constants, and all methods
- [Colors](colors.md) -- Hex validation, normalization, RGB parsing, ANSI 256 conversion
- [HSL Color Space](hsl.md) -- HSL type, conversions, and manipulation methods
- [API Reference](api-reference.md) -- Complete listing of all exported symbols
