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

## Quick Start

### Build a palette and read colors

Create a `Palette` with a name, category, and `Colors` map of semantic keys to hex values. Call `Validate()` to confirm it's complete, then use `GetOrDefault(key, fallback)` to read colors safely.

### Merge and clone

`Clone()` creates a deep copy. `Merge(other, overwrite)` copies colors from another palette — pass `false` to preserve existing keys, `true` to let the other palette override them.

### HSL color manipulation

Convert a hex color to `HSL` with `HexToHSL`, then use `Lighten`, `Darken`, `Rotate`, `Saturate`, or `Desaturate` to adjust it. Convert back to hex with `ToHex()`.

### Extract terminal colors

`ToTerminalColors()` maps semantic palette colors to the ANSI 16-color terminal set using fallback chains. `TerminalPalette()` returns a new palette containing only the terminal-relevant colors, named `<original-name>-terminal`.

## Further Reading

- [Palette](palette.md) -- `Palette` struct, color key constants, and all methods
- [Colors](colors.md) -- Hex validation, normalization, RGB parsing, ANSI 256 conversion
- [HSL Color Space](hsl.md) -- HSL type, conversions, and manipulation methods
- [API Reference](api-reference.md) -- Complete listing of all exported symbols
