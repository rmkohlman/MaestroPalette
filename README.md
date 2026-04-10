# MaestroPalette

A Go package providing shared color palette primitives for the DevOpsMaestro ecosystem.

## What It Provides

- `Palette` struct with semantic color keys, YAML/JSON serialization, and methods for get, set, merge, clone, and validation
- 55 semantic color key constants spanning backgrounds, foregrounds, UI elements, diagnostics, accents, standard names, and ANSI terminal colors
- Hex color utilities: validation, normalization, RGB parsing, ANSI 256-color conversion
- HSL color space support: conversion to/from hex, and operations for lightening, darkening, rotating hue, saturating, and desaturating
- Terminal color extraction: mapping semantic palette colors to the ANSI 16-color set with fallback chains

Zero external Go dependencies. Pure stdlib.

## Usage

### Building and using a Palette

Create a `Palette` with a name, category, and `Colors` map of semantic keys to hex values. Use `GetOrDefault(key, fallback)` to read colors safely. `Merge(other, overwrite)` copies colors from another palette — pass `false` to preserve existing keys. `Clone()` creates a deep copy.

### Working with HSL

Convert a hex color to `HSL` with `HexToHSL`, manipulate it with `Lighten`, `Darken`, `Rotate`, `Saturate`, or `Desaturate`, then convert back to hex with `ToHex()`.

### Extracting terminal colors

`ToTerminalColors()` maps semantic palette colors to the ANSI 16-color terminal set using fallback chains. `TerminalPalette()` returns a new palette containing only terminal-relevant colors.

## Documentation

For comprehensive documentation, see the [MaestroPalette Documentation](https://rmkohlman.github.io/MaestroPalette/).

## License

GPL-3.0 with commercial dual-license. See [LICENSE](LICENSE) for details.

## Part of the DevOpsMaestro Ecosystem

MaestroPalette is a shared primitive used by [DevOpsMaestro](https://github.com/rmkohlman/devopsmaestro) and related tools.
