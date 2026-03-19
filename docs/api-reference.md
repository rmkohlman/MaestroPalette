# API Reference

Complete listing of all exported types, constants, variables, and functions in package `palette` (`github.com/rmkohlman/MaestroPalette`).

---

## Types

### Palette

```go
type Palette struct {
    Name         string            `yaml:"name"                    json:"name"`
    Description  string            `yaml:"description,omitempty"   json:"description,omitempty"`
    Author       string            `yaml:"author,omitempty"        json:"author,omitempty"`
    Category     Category          `yaml:"category,omitempty"      json:"category,omitempty"`
    Colors       map[string]string `yaml:"colors,omitempty"        json:"colors,omitempty"`
    PromptColors map[string]string `yaml:"promptColors,omitempty"  json:"promptColors,omitempty"`
}
```

Core type. Holds a named map of hex color values keyed by semantic string constants.

### Category

```go
type Category string
```

Represents the type of a color palette.

### HSL

```go
type HSL struct {
    H float64 // Hue: 0-360 degrees
    S float64 // Saturation: 0-1 (0% to 100%)
    L float64 // Lightness: 0-1 (0% to 100%)
}
```

Represents a color in the HSL color space.

---

## Constants

### Category constants

```go
const (
    CategoryDark  Category = "dark"
    CategoryLight Category = "light"
    CategoryBoth  Category = "both"
)
```

### Background color keys

```go
const (
    ColorBg          = "bg"
    ColorBgDark      = "bg_dark"
    ColorBgHighlight = "bg_highlight"
    ColorBgSearch    = "bg_search"
    ColorBgVisual    = "bg_visual"
    ColorBgFloat     = "bg_float"
    ColorBgPopup     = "bg_popup"
    ColorBgSidebar   = "bg_sidebar"
    ColorBgStatusline = "bg_statusline"
)
```

### Foreground color keys

```go
const (
    ColorFg        = "fg"
    ColorFgDark    = "fg_dark"
    ColorFgGutter  = "fg_gutter"
    ColorFgSidebar = "fg_sidebar"
)
```

### UI element color keys

```go
const (
    ColorBorder  = "border"
    ColorComment = "comment"
)
```

### Diagnostic/status color keys

```go
const (
    ColorError   = "error"
    ColorWarning = "warning"
    ColorInfo    = "info"
    ColorHint    = "hint"
    ColorSuccess = "success"
)
```

### Accent color keys

```go
const (
    ColorPrimary   = "primary"
    ColorSecondary = "secondary"
    ColorAccent    = "accent"
)
```

### Standard color name keys

```go
const (
    ColorBlack   = "black"
    ColorRed     = "red"
    ColorGreen   = "green"
    ColorYellow  = "yellow"
    ColorBlue    = "blue"
    ColorMagenta = "magenta"
    ColorCyan    = "cyan"
    ColorWhite   = "white"
    ColorOrange  = "orange"
    ColorPurple  = "purple"
    ColorTeal    = "teal"
    ColorPink    = "pink"
)
```

### Terminal ANSI color keys (normal, 0-7)

```go
const (
    TermBlack   = "ansi_black"
    TermRed     = "ansi_red"
    TermGreen   = "ansi_green"
    TermYellow  = "ansi_yellow"
    TermBlue    = "ansi_blue"
    TermMagenta = "ansi_magenta"
    TermCyan    = "ansi_cyan"
    TermWhite   = "ansi_white"
)
```

### Terminal ANSI color keys (bright, 8-15)

```go
const (
    TermBrightBlack   = "ansi_bright_black"
    TermBrightRed     = "ansi_bright_red"
    TermBrightGreen   = "ansi_bright_green"
    TermBrightYellow  = "ansi_bright_yellow"
    TermBrightBlue    = "ansi_bright_blue"
    TermBrightMagenta = "ansi_bright_magenta"
    TermBrightCyan    = "ansi_bright_cyan"
    TermBrightWhite   = "ansi_bright_white"
)
```

### Terminal UI color keys

```go
const (
    TermCursor        = "cursor"
    TermCursorText    = "cursor_text"
    TermSelection     = "selection"
    TermSelectionText = "selection_text"
)
```

---

## Variables

### RequiredColors

```go
var RequiredColors = []string{
    ColorBg,
    ColorFg,
    ColorError,
    ColorWarning,
    ColorInfo,
}
```

The five color keys considered the minimum for a complete theme.

---

## Functions

### AllColorKeys

```go
func AllColorKeys() []string
```

Returns a slice of all 23 semantic color key constants (backgrounds, foregrounds, UI elements, diagnostics, accents). Does not include standard color names (`ColorBlack`, `ColorRed`, etc.) or terminal ANSI keys (`TermBlack`, etc.).

### IsValidHexColor

```go
func IsValidHexColor(color string) bool
```

Returns `true` if `color` is a valid hex color in `#RGB`, `#RRGGBB`, or `#RRGGBBAA` format (case-insensitive).

### NormalizeHexColor

```go
func NormalizeHexColor(color string) string
```

Normalizes a valid hex color to lowercase `#RRGGBB`:
- `#RGB` is expanded by doubling each digit.
- `#RRGGBBAA` has the alpha channel stripped.
- An invalid input is returned unchanged.

### ParseRGB

```go
func ParseRGB(color string) (r, g, b int, err error)
```

Returns the red, green, and blue integer components (0-255) of a hex color. Returns an error if `color` is not a valid hex color.

### ToRGBString

```go
func ToRGBString(color string) (string, error)
```

Returns a CSS-style string like `"rgb(255, 128, 64)"`. Returns an error if `color` is not a valid hex color.

### ToANSI256

```go
func ToANSI256(color string) (int, error)
```

Returns the nearest ANSI 256-color palette index for the given hex color. Returns an error if `color` is not a valid hex color.

### HexToHSL

```go
func HexToHSL(hex string) (HSL, error)
```

Converts a hex color string to an `HSL` value. Returns an error if `hex` is not valid.

### HexToRGB

```go
func HexToRGB(hex string) (r, g, b uint8, err error)
```

Converts a hex color string to `uint8` RGB components (0-255). Returns an error if `hex` is not valid.

### RGBToHSL

```go
func RGBToHSL(r, g, b uint8) HSL
```

Converts `uint8` RGB components to an `HSL` value.

### ParseHSL

```go
func ParseHSL(s string) (HSL, error)
```

Parses an HSL string. Accepts `hsl(H, S%, L%)` or `H,S,L` (decimal) format. Returns an error if the format is not recognized or values are non-numeric.

---

## Palette Methods

### Get

```go
func (p *Palette) Get(key string) string
```

Returns the color for `key`, or `""` if not found or `Colors` is nil.

### GetOrDefault

```go
func (p *Palette) GetOrDefault(key, defaultColor string) string
```

Returns the color for `key` if present and non-empty. Returns `defaultColor` otherwise.

### Set

```go
func (p *Palette) Set(key, value string)
```

Sets the color for `key`. Initializes `Colors` if nil.

### Has

```go
func (p *Palette) Has(key string) bool
```

Returns `true` if `key` exists in `Colors`.

### GetPromptColor

```go
func (p *Palette) GetPromptColor(key string) string
```

Returns the prompt color override for `key`, or `""` if absent.

### HasPromptColors

```go
func (p *Palette) HasPromptColors() bool
```

Returns `true` if `PromptColors` is non-nil and non-empty.

### Validate

```go
func (p *Palette) Validate() error
```

Returns an error if `Name` is empty, any color value fails `IsValidHexColor`, or `Category` is an unrecognized value.

### Merge

```go
func (p *Palette) Merge(other *Palette, overwrite bool)
```

Copies colors from `other` into `p`. When `overwrite` is `false`, existing keys in `p` are not replaced.

### Clone

```go
func (p *Palette) Clone() *Palette
```

Returns a deep copy of the palette. Returns `nil` if called on a nil `*Palette`.

### ToTerminalColors

```go
func (p *Palette) ToTerminalColors() map[string]string
```

Returns a `map[string]string` mapping ANSI 16-color terminal keys to hex values, resolved from the semantic color map using fallback chains. Empty entries are omitted. Returns `nil` for a nil `*Palette`; returns an empty non-nil map if `Colors` is nil.

### TerminalPalette

```go
func (p *Palette) TerminalPalette() *Palette
```

Returns a new `Palette` containing only terminal-relevant colors. Name is set to `<original-name>-terminal`. Returns `nil` for a nil `*Palette`.

---

## HSL Methods

### ToHex

```go
func (h HSL) ToHex() string
```

Returns the lowercase `#rrggbb` hex string for the HSL color.

### ToRGB

```go
func (h HSL) ToRGB() (r, g, b uint8)
```

Returns `uint8` RGB components (0-255).

### String

```go
func (h HSL) String() string
```

Returns `"hsl(H, S%, L%)"` with one decimal place per component.

### Shift

```go
func (h HSL) Shift(hDelta, sDelta, lDelta float64) HSL
```

Applies deltas to all three components. Hue is normalized to 0-360; saturation and lightness are clamped to 0-1.

### Lighten

```go
func (h HSL) Lighten(amount float64) HSL
```

Returns a new HSL with `L` increased by `amount` (clamped to 1).

### Darken

```go
func (h HSL) Darken(amount float64) HSL
```

Returns a new HSL with `L` decreased by `amount` (clamped to 0).

### Saturate

```go
func (h HSL) Saturate(amount float64) HSL
```

Returns a new HSL with `S` increased by `amount` (clamped to 1).

### Desaturate

```go
func (h HSL) Desaturate(amount float64) HSL
```

Returns a new HSL with `S` decreased by `amount` (clamped to 0).

### Rotate

```go
func (h HSL) Rotate(degrees float64) HSL
```

Returns a new HSL with `H` rotated by `degrees`. Normalized to 0-360.

### WithHue

```go
func (h HSL) WithHue(hue float64) HSL
```

Returns a new HSL with `H` set to the given absolute value, normalized to 0-360.

### WithSaturation

```go
func (h HSL) WithSaturation(saturation float64) HSL
```

Returns a new HSL with `S` set to the given absolute value, clamped to 0-1.

### WithLightness

```go
func (h HSL) WithLightness(lightness float64) HSL
```

Returns a new HSL with `L` set to the given absolute value, clamped to 0-1.
