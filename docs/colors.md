# Colors

The `colors.go` file provides hex color validation, normalization, RGB parsing, ANSI 256-color conversion, and the `Palette.Validate` method.

## Hex Color Format

The package accepts hex colors in three formats:

| Format | Example | Notes |
|---|---|---|
| `#RGB` | `#fff` | 3-digit shorthand |
| `#RRGGBB` | `#1a1b26` | Standard 6-digit |
| `#RRGGBBAA` | `#1a1b26ff` | 8-digit with alpha channel |

The `#` prefix is required. Matching is case-insensitive.

## Functions

### IsValidHexColor

```go
func IsValidHexColor(color string) bool
```

Returns `true` if `color` matches `#RGB`, `#RRGGBB`, or `#RRGGBBAA` (case-insensitive). Returns `false` for any other input including empty strings, colors without the `#` prefix, and formats like `rgb(...)`.

```go
palette.IsValidHexColor("#1a1b26")   // true
palette.IsValidHexColor("#fff")      // true
palette.IsValidHexColor("#FFF")      // true
palette.IsValidHexColor("#1a1b26ff") // true  (with alpha)
palette.IsValidHexColor("1a1b26")    // false (missing #)
palette.IsValidHexColor("#gg0000")   // false (invalid hex digits)
palette.IsValidHexColor("")          // false
```

### NormalizeHexColor

```go
func NormalizeHexColor(color string) string
```

Normalizes a valid hex color to lowercase `#RRGGBB` format:

- `#RGB` is expanded to `#RRGGBB` by doubling each digit.
- `#RRGGBBAA` is reduced to `#RRGGBB` by stripping the alpha channel.
- `#RRGGBB` is returned lowercased and otherwise unchanged.
- An invalid input is returned unchanged (no error).

```go
palette.NormalizeHexColor("#FFF")      // "#ffffff"
palette.NormalizeHexColor("#fff")      // "#ffffff"
palette.NormalizeHexColor("#1a1b26")   // "#1a1b26"
palette.NormalizeHexColor("#1a1b26ff") // "#1a1b26"
palette.NormalizeHexColor("invalid")   // "invalid"
```

### ParseRGB

```go
func ParseRGB(color string) (r, g, b int, err error)
```

Extracts the red, green, and blue integer components (each in the range 0-255) from a hex color string. Normalizes the input before parsing so `#RGB` and `#RRGGBBAA` inputs are handled correctly.

Returns an error if `color` is not a valid hex color.

```go
r, g, b, err := palette.ParseRGB("#1a1b26")
// r=26, g=27, b=38, err=nil

r, g, b, err = palette.ParseRGB("#ff8040")
// r=255, g=128, b=64, err=nil

_, _, _, err = palette.ParseRGB("invalid")
// err != nil
```

### ToRGBString

```go
func ToRGBString(color string) (string, error)
```

Converts a hex color to a CSS-style RGB string. Returns an error if `color` is not a valid hex color.

```go
s, err := palette.ToRGBString("#ff8040")
// s == "rgb(255, 128, 64)", err == nil
```

### ToANSI256

```go
func ToANSI256(color string) (int, error)
```

Converts a hex color to the nearest ANSI 256-color palette index. Returns an error if `color` is not a valid hex color.

The algorithm:

1. If all three RGB components are equal (grayscale):
   - Component value `< 8` maps to index `16` (black).
   - Component value `> 248` maps to index `231` (white).
   - Otherwise maps to the grayscale ramp at indices 232-255 using `int((r-8)/10) + 232`.
2. All other colors map to the 6x6x6 color cube at indices 16-231 using `16 + (36 * rIdx) + (6 * gIdx) + bIdx`, where each index is `int(channel/255.0 * 5.0)`.

```go
code, err := palette.ToANSI256("#000000") // 16
code, err  = palette.ToANSI256("#ffffff") // 231
code, err  = palette.ToANSI256("#808080") // 244 (grayscale ramp)
```

## Palette.Validate

```go
func (p *Palette) Validate() error
```

Validates the palette using `IsValidHexColor` for each color value. See [Palette - Validate](palette.md#validate) for full details.

## RequiredColors and AllColorKeys

```go
var RequiredColors = []string{
    "bg", "fg", "error", "warning", "info",
}

func AllColorKeys() []string
```

`RequiredColors` lists the five keys considered the minimum for a complete theme. `AllColorKeys` returns all 23 semantic color key constants. Neither includes standard color names (`"red"`, `"blue"`, etc.) or ANSI terminal keys (`"ansi_red"`, etc.).
