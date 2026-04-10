# Colors

Hex color validation, normalization, RGB parsing, and ANSI 256-color conversion utilities.

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

Returns `true` if `color` matches `#RGB`, `#RRGGBB`, or `#RRGGBBAA` (case-insensitive). Returns `false` for any other input including empty strings, colors without the `#` prefix, and formats like `rgb(...)`.

| Input | Result |
|-------|--------|
| `"#1a1b26"` | `true` |
| `"#fff"` | `true` |
| `"#FFF"` | `true` |
| `"#1a1b26ff"` | `true` (with alpha) |
| `"1a1b26"` | `false` (missing `#`) |
| `"#gg0000"` | `false` (invalid hex digits) |
| `""` | `false` |

### NormalizeHexColor

Normalizes a valid hex color to lowercase `#RRGGBB` format:

- `#RGB` is expanded to `#RRGGBB` by doubling each digit.
- `#RRGGBBAA` is reduced to `#RRGGBB` by stripping the alpha channel.
- `#RRGGBB` is returned lowercased and otherwise unchanged.
- An invalid input is returned unchanged (no error).

| Input | Output |
|-------|--------|
| `"#FFF"` | `"#ffffff"` |
| `"#fff"` | `"#ffffff"` |
| `"#1a1b26"` | `"#1a1b26"` |
| `"#1a1b26ff"` | `"#1a1b26"` |
| `"invalid"` | `"invalid"` |

### ParseRGB

Extracts the red, green, and blue integer components (each in the range 0-255) from a hex color string. Normalizes the input before parsing so `#RGB` and `#RRGGBBAA` inputs are handled correctly. Returns an error if `color` is not a valid hex color.

- `"#1a1b26"` → r=26, g=27, b=38
- `"#ff8040"` → r=255, g=128, b=64

### ToRGBString

Converts a hex color to a CSS-style RGB string. Returns an error if `color` is not a valid hex color.

- `"#ff8040"` → `"rgb(255, 128, 64)"`

### ToANSI256

Converts a hex color to the nearest ANSI 256-color palette index. Returns an error if `color` is not a valid hex color.

The algorithm:

1. If all three RGB components are equal (grayscale):
   - Component value `< 8` maps to index `16` (black).
   - Component value `> 248` maps to index `231` (white).
   - Otherwise maps to the grayscale ramp at indices 232-255.
2. All other colors map to the 6x6x6 color cube at indices 16-231.

| Input | Output |
|-------|--------|
| `"#000000"` | `16` |
| `"#ffffff"` | `231` |
| `"#808080"` | `244` (grayscale ramp) |

## Palette.Validate

Validates the palette using `IsValidHexColor` for each color value. See [Palette - Validate](palette.md#validate) for full details.

## RequiredColors and AllColorKeys

`RequiredColors` lists the five keys considered the minimum for a complete theme: `"bg"`, `"fg"`, `"error"`, `"warning"`, `"info"`.

`AllColorKeys()` returns all 23 semantic color key constants. Neither includes standard color names (`"red"`, `"blue"`, etc.) or ANSI terminal keys (`"ansi_red"`, etc.).
