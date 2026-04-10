# Palette

The `Palette` struct is the core type in MaestroPalette. It holds a named set of hex color values keyed by semantic string constants, along with metadata fields and optional prompt color overrides.

## Fields

| Field | Type | Description |
|-------|------|-------------|
| `Name` | `string` | Palette name (required by `Validate`) |
| `Description` | `string` | Human-readable description (optional) |
| `Author` | `string` | Author name (optional) |
| `Category` | `Category` | `"dark"`, `"light"`, or `"both"` (optional) |
| `Colors` | `map[string]string` | Semantic color key → hex value |
| `PromptColors` | `map[string]string` | Optional overrides for Starship prompt segments |

Colors are stored as hex strings (e.g., `"#1a1b26"`). The map keys are the semantic color key constants defined in this package.

`PromptColors` provides optional overrides for Starship prompt segments. Keys are Catppuccin-style names (`red`, `peach`, `sky`, etc.) and values are hex colors. When present, these override the default segment color mappings when generating a `starship.toml` palette.

## Category

`Category` is one of `"dark"`, `"light"`, or `"both"`. `Validate` enforces that `Category` is one of these three values if set.

## Methods

### Get

Returns the color value for `key`, or an empty string if the key is not present or `Colors` is nil.

### GetOrDefault

Returns the color value for `key` if it exists and is non-empty. Returns `defaultColor` otherwise.

### Set

Sets the color value for `key`. Initializes the `Colors` map if it is nil.

### Has

Returns `true` if `key` exists in `Colors`, regardless of the value. Returns `false` if `Colors` is nil.

### GetPromptColor

Returns the prompt color override for `key`, or an empty string if `PromptColors` is nil or the key is absent.

### HasPromptColors

Returns `true` if `PromptColors` is non-nil and has at least one entry.

### Validate

Checks that:

- `Name` is non-empty
- Every non-empty color value in `Colors` passes `IsValidHexColor`
- `Category`, if set, is one of `"dark"`, `"light"`, or `"both"`

Returns a descriptive error on the first violation found, or `nil` if valid.

### Merge

Copies all color entries from `other.Colors` into `Colors`. If `overwrite` is `false`, existing keys are not replaced. If `other` is nil or `other.Colors` is nil, Merge is a no-op. Initializes `Colors` if nil.

- `Merge(overlay, false)` — adds colors from overlay without replacing existing ones
- `Merge(overlay, true)` — replaces all conflicting keys with overlay values

### Clone

Returns a deep copy of the palette. All fields are copied. Both `Colors` and `PromptColors` maps are duplicated so mutations to the clone do not affect the original. Returns `nil` if called on a nil palette.

### ToTerminalColors

Extracts an ANSI 16-color terminal palette from the semantic palette colors. Returns a `map[string]string` keyed by the `Term*` and `Color{Bg,Fg}` constants. Each terminal slot is resolved through a fallback chain — for example `TermRed` resolves the first non-empty value found among keys `"red"` then `"error"`.

Empty-valued entries are removed from the result. Returns `nil` if called on a nil palette. Returns an empty (non-nil) map if `Colors` is nil.

See [Terminal color fallback chains](#terminal-color-fallback-chains) below for the full mapping.

### TerminalPalette

Creates a new `Palette` containing only terminal-relevant colors. The new palette has the same `Description`, `Author`, and `Category` as the receiver; `Name` is set to `<original-name>-terminal`; and `Colors` is the result of `ToTerminalColors()`. Returns `nil` if called on a nil palette.

## Terminal Color Fallback Chains

`ToTerminalColors` resolves each terminal slot by checking keys in order and using the first non-empty value found.

| Terminal key | Fallback chain |
|---|---|
| `ansi_black` | `"black"`, `"bg_dark"`, `"bg"` |
| `ansi_red` | `"red"`, `"error"` |
| `ansi_green` | `"green"`, `"success"` |
| `ansi_yellow` | `"yellow"`, `"warning"` |
| `ansi_blue` | `"blue"`, `"info"`, `"primary"` |
| `ansi_magenta` | `"magenta"`, `"purple"`, `"pink"` |
| `ansi_cyan` | `"cyan"`, `"teal"` |
| `ansi_white` | `"white"`, `"fg"` |
| `ansi_bright_black` | `"bright_black"`, `"comment"`, `"fg_gutter"` |
| `ansi_bright_red` | `"bright_red"`, `"red"`, `"error"` |
| `ansi_bright_green` | `"bright_green"`, `"green"`, `"success"` |
| `ansi_bright_yellow` | `"bright_yellow"`, `"yellow"`, `"orange"`, `"warning"` |
| `ansi_bright_blue` | `"bright_blue"`, `"blue"`, `"info"` |
| `ansi_bright_magenta` | `"bright_magenta"`, `"magenta"`, `"purple"`, `"lavender"` |
| `ansi_bright_cyan` | `"bright_cyan"`, `"cyan"`, `"teal"`, `"sky"` |
| `ansi_bright_white` | `"bright_white"`, `"fg_dark"`, `"fg"` |
| `bg` | `"bg"` |
| `fg` | `"fg"` |
| `cursor` | `"cursor"`, `"fg"` |
| `cursor_text` | `"cursor_text"`, `"bg"` |
| `selection` | `"selection"`, `"bg_visual"`, `"bg_highlight"` |
| `selection_text` | `"selection_text"`, `"fg"` |

## Color Key Constants

All color key constants are untyped string constants. They are organized into groups.

### Background colors

| Constant | Value | Description |
|---|---|---|
| `ColorBg` | `"bg"` | Primary background color |
| `ColorBgDark` | `"bg_dark"` | Darker background variant |
| `ColorBgHighlight` | `"bg_highlight"` | Background for highlighted elements |
| `ColorBgSearch` | `"bg_search"` | Background for search matches |
| `ColorBgVisual` | `"bg_visual"` | Background for visual selections |
| `ColorBgFloat` | `"bg_float"` | Background for floating windows/panels |
| `ColorBgPopup` | `"bg_popup"` | Background for popups/menus |
| `ColorBgSidebar` | `"bg_sidebar"` | Background for sidebars |
| `ColorBgStatusline` | `"bg_statusline"` | Background for status lines |

### Foreground colors

| Constant | Value | Description |
|---|---|---|
| `ColorFg` | `"fg"` | Primary foreground/text color |
| `ColorFgDark` | `"fg_dark"` | Darker foreground variant |
| `ColorFgGutter` | `"fg_gutter"` | Foreground for gutter/line numbers |
| `ColorFgSidebar` | `"fg_sidebar"` | Foreground for sidebar text |

### UI element colors

| Constant | Value | Description |
|---|---|---|
| `ColorBorder` | `"border"` | Color for borders |
| `ColorComment` | `"comment"` | Color for comments/muted text |

### Diagnostic/status colors

| Constant | Value | Description |
|---|---|---|
| `ColorError` | `"error"` | Color for errors |
| `ColorWarning` | `"warning"` | Color for warnings |
| `ColorInfo` | `"info"` | Color for informational messages |
| `ColorHint` | `"hint"` | Color for hints |
| `ColorSuccess` | `"success"` | Color for success states |

### Accent colors

| Constant | Value | Description |
|---|---|---|
| `ColorPrimary` | `"primary"` | Primary accent color |
| `ColorSecondary` | `"secondary"` | Secondary accent color |
| `ColorAccent` | `"accent"` | General accent color |

### Standard color names

These are the standard color names found in theme files that map to terminal ANSI colors.

| Constant | Value |
|---|---|
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

### Terminal ANSI color keys

These are the keys used in the map returned by `ToTerminalColors` and `TerminalPalette`.

#### Normal colors (ANSI 0-7)

| Constant | Value |
|---|---|
| `TermBlack` | `"ansi_black"` |
| `TermRed` | `"ansi_red"` |
| `TermGreen` | `"ansi_green"` |
| `TermYellow` | `"ansi_yellow"` |
| `TermBlue` | `"ansi_blue"` |
| `TermMagenta` | `"ansi_magenta"` |
| `TermCyan` | `"ansi_cyan"` |
| `TermWhite` | `"ansi_white"` |

#### Bright colors (ANSI 8-15)

| Constant | Value |
|---|---|
| `TermBrightBlack` | `"ansi_bright_black"` |
| `TermBrightRed` | `"ansi_bright_red"` |
| `TermBrightGreen` | `"ansi_bright_green"` |
| `TermBrightYellow` | `"ansi_bright_yellow"` |
| `TermBrightBlue` | `"ansi_bright_blue"` |
| `TermBrightMagenta` | `"ansi_bright_magenta"` |
| `TermBrightCyan` | `"ansi_bright_cyan"` |
| `TermBrightWhite` | `"ansi_bright_white"` |

#### Terminal UI colors

| Constant | Value | Description |
|---|---|---|
| `TermCursor` | `"cursor"` | Cursor color |
| `TermCursorText` | `"cursor_text"` | Text color under cursor |
| `TermSelection` | `"selection"` | Selection background |
| `TermSelectionText` | `"selection_text"` | Text color in selection |

## AllColorKeys and RequiredColors

`AllColorKeys()` returns a slice of all 23 semantic color key constants (backgrounds, foregrounds, UI, diagnostics, accents). Does not include standard color names or terminal ANSI keys.

`RequiredColors` is a package-level variable listing the five color keys considered the minimum for a complete theme: `ColorBg`, `ColorFg`, `ColorError`, `ColorWarning`, `ColorInfo`.
