# API Reference

Complete listing of all exported types, constants, variables, and functions in package `palette` (`github.com/rmkohlman/MaestroPalette`).

---

## Types

### Palette

Core type. Holds a named map of hex color values keyed by semantic string constants.

| Field | Type | Description |
|-------|------|-------------|
| `Name` | `string` | Palette name (required) |
| `Description` | `string` | Human-readable description (optional) |
| `Author` | `string` | Author name (optional) |
| `Category` | `Category` | `"dark"`, `"light"`, or `"both"` (optional) |
| `Colors` | `map[string]string` | Semantic color key → hex value |
| `PromptColors` | `map[string]string` | Prompt color overrides (Catppuccin-style names) |

### Category

Represents the type of a color palette. Valid values: `"dark"`, `"light"`, `"both"`.

### HSL

Represents a color in the HSL color space.

| Field | Range | Description |
|-------|-------|-------------|
| `H` | 0–360 | Hue in degrees |
| `S` | 0–1 | Saturation (0% to 100%) |
| `L` | 0–1 | Lightness (0% to 100%) |

---

## Constants

### Category constants

| Constant | Value |
|----------|-------|
| `CategoryDark` | `"dark"` |
| `CategoryLight` | `"light"` |
| `CategoryBoth` | `"both"` |

### Background color keys

| Constant | Value |
|----------|-------|
| `ColorBg` | `"bg"` |
| `ColorBgDark` | `"bg_dark"` |
| `ColorBgHighlight` | `"bg_highlight"` |
| `ColorBgSearch` | `"bg_search"` |
| `ColorBgVisual` | `"bg_visual"` |
| `ColorBgFloat` | `"bg_float"` |
| `ColorBgPopup` | `"bg_popup"` |
| `ColorBgSidebar` | `"bg_sidebar"` |
| `ColorBgStatusline` | `"bg_statusline"` |

### Foreground color keys

| Constant | Value |
|----------|-------|
| `ColorFg` | `"fg"` |
| `ColorFgDark` | `"fg_dark"` |
| `ColorFgGutter` | `"fg_gutter"` |
| `ColorFgSidebar` | `"fg_sidebar"` |

### UI element color keys

| Constant | Value |
|----------|-------|
| `ColorBorder` | `"border"` |
| `ColorComment` | `"comment"` |

### Diagnostic/status color keys

| Constant | Value |
|----------|-------|
| `ColorError` | `"error"` |
| `ColorWarning` | `"warning"` |
| `ColorInfo` | `"info"` |
| `ColorHint` | `"hint"` |
| `ColorSuccess` | `"success"` |

### Accent color keys

| Constant | Value |
|----------|-------|
| `ColorPrimary` | `"primary"` |
| `ColorSecondary` | `"secondary"` |
| `ColorAccent` | `"accent"` |

### Standard color name keys

| Constant | Value |
|----------|-------|
| `ColorBlack` | `"black"` |
| `ColorRed` | `"red"` |
| `ColorGreen` | `"green"` |
| `ColorYellow` | `"yellow"` |
| `ColorBlue` | `"blue"` |
| `ColorMagenta` | `"magenta"` |
| `ColorCyan` | `"cyan"` |
| `ColorWhite` | `"white"` |
| `ColorOrange` | `"orange"` |
| `ColorPurple` | `"purple"` |
| `ColorTeal` | `"teal"` |
| `ColorPink` | `"pink"` |

### Terminal ANSI color keys (normal, 0-7)

| Constant | Value |
|----------|-------|
| `TermBlack` | `"ansi_black"` |
| `TermRed` | `"ansi_red"` |
| `TermGreen` | `"ansi_green"` |
| `TermYellow` | `"ansi_yellow"` |
| `TermBlue` | `"ansi_blue"` |
| `TermMagenta` | `"ansi_magenta"` |
| `TermCyan` | `"ansi_cyan"` |
| `TermWhite` | `"ansi_white"` |

### Terminal ANSI color keys (bright, 8-15)

| Constant | Value |
|----------|-------|
| `TermBrightBlack` | `"ansi_bright_black"` |
| `TermBrightRed` | `"ansi_bright_red"` |
| `TermBrightGreen` | `"ansi_bright_green"` |
| `TermBrightYellow` | `"ansi_bright_yellow"` |
| `TermBrightBlue` | `"ansi_bright_blue"` |
| `TermBrightMagenta` | `"ansi_bright_magenta"` |
| `TermBrightCyan` | `"ansi_bright_cyan"` |
| `TermBrightWhite` | `"ansi_bright_white"` |

### Terminal UI color keys

| Constant | Value |
|----------|-------|
| `TermCursor` | `"cursor"` |
| `TermCursorText` | `"cursor_text"` |
| `TermSelection` | `"selection"` |
| `TermSelectionText` | `"selection_text"` |

---

## Variables

### RequiredColors

The five color keys considered the minimum for a complete theme: `ColorBg`, `ColorFg`, `ColorError`, `ColorWarning`, `ColorInfo`.

---

## Functions

| Function | Description |
|----------|-------------|
| `AllColorKeys()` | Returns a slice of all 23 semantic color key constants (backgrounds, foregrounds, UI elements, diagnostics, accents). Does not include standard color names or terminal ANSI keys. |
| `IsValidHexColor(color)` | Returns `true` if `color` is a valid hex color in `#RGB`, `#RRGGBB`, or `#RRGGBBAA` format (case-insensitive). |
| `NormalizeHexColor(color)` | Normalizes a valid hex color to lowercase `#RRGGBB`. `#RGB` is expanded by doubling each digit. `#RRGGBBAA` has the alpha channel stripped. An invalid input is returned unchanged. |
| `ParseRGB(color)` | Returns the red, green, and blue integer components (0-255). Returns an error if `color` is not a valid hex color. |
| `ToRGBString(color)` | Returns a CSS-style string like `"rgb(255, 128, 64)"`. Returns an error if not valid. |
| `ToANSI256(color)` | Returns the nearest ANSI 256-color palette index for the given hex color. Returns an error if not valid. |
| `HexToHSL(hex)` | Converts a hex color string to an `HSL` value. Returns an error if `hex` is not valid. |
| `HexToRGB(hex)` | Converts a hex color string to `uint8` RGB components. Returns an error if `hex` is not valid. |
| `RGBToHSL(r, g, b)` | Converts `uint8` RGB components to an `HSL` value. |
| `ParseHSL(s)` | Parses an HSL string. Accepts `hsl(H, S%, L%)` or `H,S,L` (decimal) format. |

---

## Palette Methods

| Method | Description |
|--------|-------------|
| `Get(key)` | Returns the color for `key`, or `""` if not found or `Colors` is nil. |
| `GetOrDefault(key, defaultColor)` | Returns the color for `key` if present and non-empty. Returns `defaultColor` otherwise. |
| `Set(key, value)` | Sets the color for `key`. Initializes `Colors` if nil. |
| `Has(key)` | Returns `true` if `key` exists in `Colors`. |
| `GetPromptColor(key)` | Returns the prompt color override for `key`, or `""` if absent. |
| `HasPromptColors()` | Returns `true` if `PromptColors` is non-nil and non-empty. |
| `Validate()` | Returns an error if `Name` is empty, any color value fails `IsValidHexColor`, or `Category` is unrecognized. |
| `Merge(other, overwrite)` | Copies colors from `other` into the palette. When `overwrite` is `false`, existing keys are not replaced. |
| `Clone()` | Returns a deep copy of the palette. Returns `nil` if called on a nil palette. |
| `ToTerminalColors()` | Returns a `map[string]string` mapping ANSI 16-color terminal keys to hex values, resolved from the semantic color map using fallback chains. |
| `TerminalPalette()` | Returns a new palette containing only terminal-relevant colors. Name is set to `<original-name>-terminal`. |

---

## HSL Methods

| Method | Description |
|--------|-------------|
| `ToHex()` | Returns the lowercase `#rrggbb` hex string for the HSL color. |
| `ToRGB()` | Returns `uint8` RGB components (0-255). |
| `String()` | Returns `"hsl(H, S%, L%)"` with one decimal place per component. |
| `Shift(hDelta, sDelta, lDelta)` | Applies deltas to all three components. Hue normalized to 0-360; saturation and lightness clamped to 0-1. |
| `Lighten(amount)` | Returns a new HSL with `L` increased by `amount` (clamped to 1). |
| `Darken(amount)` | Returns a new HSL with `L` decreased by `amount` (clamped to 0). |
| `Saturate(amount)` | Returns a new HSL with `S` increased by `amount` (clamped to 1). |
| `Desaturate(amount)` | Returns a new HSL with `S` decreased by `amount` (clamped to 0). |
| `Rotate(degrees)` | Returns a new HSL with `H` rotated by `degrees`. Normalized to 0-360. |
| `WithHue(hue)` | Returns a new HSL with `H` set to `hue`, normalized to 0-360. |
| `WithSaturation(saturation)` | Returns a new HSL with `S` set to `saturation`, clamped to 0-1. |
| `WithLightness(lightness)` | Returns a new HSL with `L` set to `lightness`, clamped to 0-1. |
