# MaestroPalette

A Go package providing shared color palette primitives for the DevOpsMaestro ecosystem.

## What It Provides

- `Palette` struct with semantic color keys, YAML/JSON serialization, and methods for get, set, merge, clone, and validation
- 55 semantic color key constants spanning backgrounds, foregrounds, UI elements, diagnostics, accents, standard names, and ANSI terminal colors
- Hex color utilities: validation, normalization, RGB parsing, ANSI 256-color conversion
- HSL color space support: conversion to/from hex, and operations for lightening, darkening, rotating hue, saturating, and desaturating
- Terminal color extraction: mapping semantic palette colors to the ANSI 16-color set with fallback chains

Zero external Go dependencies. Pure stdlib.

## Installation

```sh
go get github.com/rmkohlman/MaestroPalette
```

## Usage

### Building and using a Palette

```go
import "github.com/rmkohlman/MaestroPalette"

p := &palette.Palette{
    Name:     "my-theme",
    Category: palette.CategoryDark,
    Colors: map[string]string{
        palette.ColorBg:    "#1a1b26",
        palette.ColorFg:    "#c0caf5",
        palette.ColorError: "#f7768e",
    },
}

// Retrieve a color, or a default if missing
bg := p.GetOrDefault(palette.ColorBg, "#000000")

// Merge another palette without overwriting existing keys
p.Merge(overlay, false)

// Deep copy
clone := p.Clone()
```

### Working with HSL

```go
// Convert a hex color to HSL, manipulate it, convert back
hsl, err := palette.HexToHSL("#7aa2f7")
if err != nil {
    // handle error
}

lighter := hsl.Lighten(0.1)   // increase lightness by 10%
rotated := hsl.Rotate(30)     // rotate hue by 30 degrees
hex := rotated.ToHex()        // back to "#rrggbb"
```

### Extracting terminal colors

```go
// Map semantic palette colors to the ANSI 16-color terminal set
termColors := p.ToTerminalColors()
// termColors["ansi_red"], termColors["ansi_blue"], etc.

// Or create a new Palette containing only terminal-relevant colors
termPalette := p.TerminalPalette()
```

## Documentation

For comprehensive documentation, see the [MaestroPalette Documentation](https://rmkohlman.github.io/MaestroPalette/).

## License

GPL-3.0 with commercial dual-license. See [LICENSE](LICENSE) for details.

## Part of the DevOpsMaestro Ecosystem

MaestroPalette is a shared primitive used by [DevOpsMaestro](https://github.com/rmkohlman/devopsmaestro) and related tools.
