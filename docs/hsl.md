# HSL Color Space

MaestroPalette provides full HSL (Hue, Saturation, Lightness) color space support via the `HSL` struct and associated functions and methods. All HSL operations are non-mutating; every method returns a new `HSL` value.

## HSL Type

```go
type HSL struct {
    H float64 // Hue: 0-360 degrees
    S float64 // Saturation: 0-1 (0% to 100%)
    L float64 // Lightness: 0-1 (0% to 100%)
}
```

`H` is the hue angle in degrees (0 to 360). `S` and `L` are fractions in the range 0 to 1, representing 0% to 100%.

## Converting To and From Hex

### HexToHSL

```go
func HexToHSL(hex string) (HSL, error)
```

Converts a hex color string to an `HSL` value. Returns an error if `hex` is not a valid hex color.

```go
hsl, err := palette.HexToHSL("#FF0000")
// hsl.H == 0, hsl.S == 1.0, hsl.L == 0.5   (pure red)

hsl, err = palette.HexToHSL("#0000FF")
// hsl.H == 240, hsl.S == 1.0, hsl.L == 0.5  (pure blue)
```

### HexToRGB

```go
func HexToRGB(hex string) (r, g, b uint8, err error)
```

Converts a hex color string to `uint8` RGB components (0-255). This is the `uint8`-typed counterpart to `ParseRGB`, which returns `int`. Returns an error if `hex` is not a valid hex color.

```go
r, g, b, err := palette.HexToRGB("#1a1b26")
// r=26, g=27, b=38, err=nil
```

### HSL.ToHex

```go
func (h HSL) ToHex() string
```

Converts the `HSL` value to a lowercase `#rrggbb` hex string.

```go
hsl := palette.HSL{H: 120, S: 1.0, L: 0.5}
hsl.ToHex() // "#00ff00"
```

### HSL.ToRGB

```go
func (h HSL) ToRGB() (r, g, b uint8)
```

Converts the `HSL` value to `uint8` RGB components (0-255). For achromatic colors (S == 0), all three channels are set to `L * 255`.

## Converting RGB to HSL

### RGBToHSL

```go
func RGBToHSL(r, g, b uint8) HSL
```

Converts `uint8` RGB components (0-255) to an `HSL` value. For achromatic inputs (all channels equal), hue and saturation are both 0.

```go
hsl := palette.RGBToHSL(255, 0, 0)
// hsl.H == 0, hsl.S == 1.0, hsl.L == 0.5
```

## Parsing HSL Strings

### ParseHSL

```go
func ParseHSL(s string) (HSL, error)
```

Parses an HSL string into an `HSL` value. Two formats are accepted:

**CSS `hsl()` format** -- saturation and lightness with `%` suffix are divided by 100 to produce 0-1 fractions:

```
hsl(210, 50%, 30%)
```

**Comma-separated decimal format** -- saturation and lightness are already 0-1 fractions:

```
210,0.5,0.3
```

Returns an error if the string does not match either format or contains non-numeric values.

```go
hsl, err := palette.ParseHSL("hsl(210, 50%, 30%)")
// hsl.H == 210, hsl.S == 0.5, hsl.L == 0.3

hsl, err = palette.ParseHSL("210,0.5,0.3")
// hsl.H == 210, hsl.S == 0.5, hsl.L == 0.3
```

## String Representation

### HSL.String

```go
func (h HSL) String() string
```

Returns a CSS-style string representation: `hsl(H, S%, L%)` with one decimal place for each component. Saturation and lightness are multiplied by 100 for display.

```go
hsl := palette.HSL{H: 210.0, S: 0.5, L: 0.3}
hsl.String() // "hsl(210.0, 50.0%, 30.0%)"
```

## Manipulation Methods

All manipulation methods return a new `HSL` value. The receiver is not modified. Hue is normalized to the 0-360 range; saturation and lightness are clamped to 0-1.

### Shift

```go
func (h HSL) Shift(hDelta, sDelta, lDelta float64) HSL
```

Applies arbitrary deltas to all three components simultaneously. Hue wrapping and clamping are applied as described above.

```go
blue := palette.HSL{H: 240, S: 1.0, L: 0.5}
shifted := blue.Shift(-120, 0, 0)
// shifted.H == 120 (now green)

wrapped := blue.Shift(150, 0, 0)
// 240 + 150 = 390, normalized to 30
// wrapped.H == 30
```

### Lighten

```go
func (h HSL) Lighten(amount float64) HSL
```

Returns a new `HSL` with `L` increased by `amount`. Equivalent to `Shift(0, 0, amount)`. Result is clamped to 1.

```go
hsl.Lighten(0.1) // lightness + 0.1
```

### Darken

```go
func (h HSL) Darken(amount float64) HSL
```

Returns a new `HSL` with `L` decreased by `amount`. Equivalent to `Shift(0, 0, -amount)`. Result is clamped to 0.

```go
hsl.Darken(0.1) // lightness - 0.1
```

### Saturate

```go
func (h HSL) Saturate(amount float64) HSL
```

Returns a new `HSL` with `S` increased by `amount`. Equivalent to `Shift(0, amount, 0)`. Result is clamped to 1.

```go
hsl.Saturate(0.2) // saturation + 0.2
```

### Desaturate

```go
func (h HSL) Desaturate(amount float64) HSL
```

Returns a new `HSL` with `S` decreased by `amount`. Equivalent to `Shift(0, -amount, 0)`. Result is clamped to 0.

```go
hsl.Desaturate(0.2) // saturation - 0.2
```

### Rotate

```go
func (h HSL) Rotate(degrees float64) HSL
```

Returns a new `HSL` with the hue rotated by `degrees`. Equivalent to `Shift(degrees, 0, 0)`. The resulting hue is normalized to 0-360.

```go
blue := palette.HSL{H: 240, S: 1.0, L: 0.5}
blue.Rotate(120).ToHex() // "#ff0000" (red)
blue.Rotate(-120).ToHex() // "#00ff00" (green)
```

## WithHue, WithSaturation, WithLightness

These methods return a new `HSL` with one component replaced by an absolute value rather than a delta.

### WithHue

```go
func (h HSL) WithHue(hue float64) HSL
```

Returns a new `HSL` with `H` set to `hue`. The value is normalized to 0-360.

### WithSaturation

```go
func (h HSL) WithSaturation(saturation float64) HSL
```

Returns a new `HSL` with `S` set to `saturation`. The value is clamped to 0-1.

### WithLightness

```go
func (h HSL) WithLightness(lightness float64) HSL
```

Returns a new `HSL` with `L` set to `lightness`. The value is clamped to 0-1.

## Example: Complementary Color

```go
hsl, err := palette.HexToHSL("#7aa2f7")
if err != nil {
    // handle error
}

// Complementary color is 180 degrees away
complement := hsl.Rotate(180)
complementHex := complement.ToHex()
```

## Example: Generating a Shade Range

```go
hsl, err := palette.HexToHSL("#1a1b26")
if err != nil {
    // handle error
}

shades := []string{
    hsl.Lighten(0.10).ToHex(),
    hsl.Lighten(0.20).ToHex(),
    hsl.Lighten(0.30).ToHex(),
}
```
