# HSL Color Space

MaestroPalette provides full HSL (Hue, Saturation, Lightness) color space support via the `HSL` struct and associated functions and methods. All HSL operations are non-mutating; every method returns a new `HSL` value.

## HSL Type

The `HSL` struct represents a color in the HSL color space:

| Field | Range | Description |
|-------|-------|-------------|
| `H` | 0–360 | Hue angle in degrees |
| `S` | 0–1 | Saturation (0% to 100%) |
| `L` | 0–1 | Lightness (0% to 100%) |

For achromatic colors (S == 0), hue and saturation are both 0.

## Converting To and From Hex

### HexToHSL

Converts a hex color string to an `HSL` value. Returns an error if `hex` is not a valid hex color.

- `"#FF0000"` → H=0, S=1.0, L=0.5 (pure red)
- `"#0000FF"` → H=240, S=1.0, L=0.5 (pure blue)

### HexToRGB

Converts a hex color string to `uint8` RGB components (0-255). This is the `uint8`-typed counterpart to `ParseRGB`, which returns `int`. Returns an error if `hex` is not a valid hex color.

### HSL.ToHex

Converts the `HSL` value to a lowercase `#rrggbb` hex string.

- `HSL{H: 120, S: 1.0, L: 0.5}` → `"#00ff00"`

### HSL.ToRGB

Converts the `HSL` value to `uint8` RGB components (0-255). For achromatic colors (S == 0), all three channels are set to `L * 255`.

## Converting RGB to HSL

### RGBToHSL

Converts `uint8` RGB components (0-255) to an `HSL` value. For achromatic inputs (all channels equal), hue and saturation are both 0.

- `(255, 0, 0)` → H=0, S=1.0, L=0.5

## Parsing HSL Strings

### ParseHSL

Parses an HSL string into an `HSL` value. Two formats are accepted:

**CSS `hsl()` format** — saturation and lightness with `%` suffix are divided by 100 to produce 0-1 fractions:

```
hsl(210, 50%, 30%)
```

**Comma-separated decimal format** — saturation and lightness are already 0-1 fractions:

```
210,0.5,0.3
```

Returns an error if the string does not match either format or contains non-numeric values.

Both `"hsl(210, 50%, 30%)"` and `"210,0.5,0.3"` produce H=210, S=0.5, L=0.3.

## String Representation

### HSL.String

Returns a CSS-style string representation: `hsl(H, S%, L%)` with one decimal place for each component. Saturation and lightness are multiplied by 100 for display.

`HSL{H: 210.0, S: 0.5, L: 0.3}` → `"hsl(210.0, 50.0%, 30.0%)"`

## Manipulation Methods

All manipulation methods return a new `HSL` value. The receiver is not modified. Hue is normalized to the 0-360 range; saturation and lightness are clamped to 0-1.

### Shift

Applies arbitrary deltas to all three components simultaneously.

- `HSL{H: 240, ...}.Shift(-120, 0, 0)` → H=120 (blue shifted to green)
- `HSL{H: 240, ...}.Shift(150, 0, 0)` → H=30 (240+150=390, normalized to 30)

### Lighten

Returns a new `HSL` with `L` increased by `amount`. Equivalent to `Shift(0, 0, amount)`. Result is clamped to 1.

### Darken

Returns a new `HSL` with `L` decreased by `amount`. Equivalent to `Shift(0, 0, -amount)`. Result is clamped to 0.

### Saturate

Returns a new `HSL` with `S` increased by `amount`. Equivalent to `Shift(0, amount, 0)`. Result is clamped to 1.

### Desaturate

Returns a new `HSL` with `S` decreased by `amount`. Equivalent to `Shift(0, -amount, 0)`. Result is clamped to 0.

### Rotate

Returns a new `HSL` with the hue rotated by `degrees`. Equivalent to `Shift(degrees, 0, 0)`. The resulting hue is normalized to 0-360.

- `HSL{H: 240, ...}.Rotate(120)` → `"#ff0000"` (red)
- `HSL{H: 240, ...}.Rotate(-120)` → `"#00ff00"` (green)

## WithHue, WithSaturation, WithLightness

These methods return a new `HSL` with one component replaced by an absolute value rather than a delta.

### WithHue

Returns a new `HSL` with `H` set to `hue`. The value is normalized to 0-360.

### WithSaturation

Returns a new `HSL` with `S` set to `saturation`. The value is clamped to 0-1.

### WithLightness

Returns a new `HSL` with `L` set to `lightness`. The value is clamped to 0-1.

## Example: Complementary Color

Convert a hex color to HSL, rotate by 180 degrees (the complementary hue), then convert back to hex.

## Example: Generating a Shade Range

Convert a base color to HSL, then call `Lighten` with increasing amounts (e.g., 0.10, 0.20, 0.30) and convert each result back to hex with `ToHex()`.
