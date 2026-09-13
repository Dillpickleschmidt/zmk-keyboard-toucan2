# Graphite on Toucan2

This configuration starts from Beekeeb's official Toucan2 main commit
`6882c95`. It replaces the earlier, incorrect original-Toucan build.

## Base layer

The display name is the vendor's `BASE`. Punctuation behaviors use neutral
symbol-pair names. The layout name appears only in source documentation, not
in the keyboard's layer or behavior labels.

The letter and punctuation positions follow the
[original Graphite ASCII layout](https://github.com/rdavison/graphite-layout#ascii-version).
The Toucan has no number row. Numbers remain on NAV, and the additional symbols
remain on SYM.

```text
Tab       B  L  D  W  Z       '  F  O  U  J  ;
Esc/Ctrl  N  R  T  S  G       Y  H  A  E  I  ,
Shift     Q  X  M  C  V       K  P  .  -  /  Esc
          Backspace NAV Space   Space SYM Enter
```

The punctuation occupies the original Graphite columns, including apostrophe on
the inner right index column and semicolon on the outer right pinky column.
Backspace is on the outer left thumb. The right thumbs are Space, SYM, and Enter
from left to right. Both layer keys and the left inner Space retain their positions.
These thumb changes apply to the base layer only. NAV, SYM, and ADJ keep their
existing thumb bindings, including outer left Super and outer right Alt. MOU
keeps its six thumb mouse buttons.

The far-left middle key uses ZMK's standard
[mod-tap](https://zmk.dev/docs/keymaps/behaviors/hold-tap#mod-tap), `&mt LCTRL ESC`.
A tap released before 200 ms sends Escape. Holding for 200 ms, or pressing
another key while it is down, activates Left Ctrl. No custom timing or
opposite-hand restriction is added to this key. Its SYM and ADJ bindings remain
dedicated Ctrl, and NAV retains Bluetooth clear. MOU inherits the base behavior.

## Home row mods

| Tap | Hold | Finger |
| --- | --- | --- |
| N | Left Super | Left pinky |
| R | Left Alt | Left ring |
| T | Left Ctrl | Left middle |
| S | Left Shift | Left index |
| H | Right Shift | Right index |
| A | Right Ctrl | Right middle |
| E | Right Alt | Right ring |
| I | Right Super | Right pinky |

The modifier arrangement is the mirrored arrangement proposed for this setup;
Graphite itself specifies letters and punctuation, not a home row mod order.

Both behaviors copy the values in
[ZMK v0.3's home row mod example](https://github.com/zmkfirmware/zmk/blob/v0.3/docs/docs/keymaps/behaviors/hold-tap.mdx):

| Property | Value |
| --- | --- |
| `flavor` | `balanced` |
| `tapping-term-ms` | 280 |
| `require-prior-idle-ms` | 150 |
| `quick-tap-ms` | 175 |
| `hold-trigger-on-release` | Enabled |
| `hold-trigger-key-positions` | Keys on the opposite half, including that half's thumbs |

The example's code specifies 150 ms for prior idle, although its explanation
mentions 125 ms. This configuration follows the code's 150 ms value.

The trigger positions come from Beekeeb's 42-key matrix order in
`boards/shields/toucan/toucan.dtsi`. The left and right behaviors are separate,
as the official example requires. The configuration adds no custom timing
algorithm, firmware patch, or external home row mod module.

Same-hand shortcuts require holding the modifier past the tapping term before
pressing the letter. A recent non-modifier key can force a home row mod to tap
through prior-idle protection. A tap followed promptly by a hold repeats the
letter. These are the documented behavior rules, not hardware-tested timing
claims for this keyboard.

## Shift pairs

The base layer implements all four of Graphite's nonstandard US Shift pairs with
[ZMK mod-morph behaviors](https://zmk.dev/docs/keymaps/behaviors/mod-morph).
Either Shift key activates the second binding, including Shift from a home row
mod. No `keep-mods` override is needed because the shifted symbol keycodes
already include Shift.

| Key | With Shift |
| --- | --- |
| Apostrophe `'` | Underscore `_` |
| Comma `,` | Question mark `?` |
| Minus `-` | Double quote `"` |
| Slash `/` | Less than `<` |
| Period `.` | Greater than `>` through the standard US keycode |
| Semicolon `;` | Colon `:` through the standard US keycode |

The host must interpret the keycodes using a US keyboard layout. A second host
remapper must not remap the Toucan's letters again. The stock SYM layer retains
its direct symbol bindings, including its ordinary Minus/Underscore key.


## Toucan2 hardware and layers

The vendor matrix, physical layout, I2C pins, reset and ready pins, Azoteq TPS43
driver, display, power settings, and dependency manifest are retained. There is
no Cirque driver. Both halves keep the vendor's 2048-byte input thread stack.

The complete display implementation matches the latest vendor main checked on
September 13, 2026. `CONFIG_TOUCAN_STATUS_SCREEN=2` selects its current default
design. This follows the latest Toucan2 source, not an attempt to reproduce an
unknown factory firmware version.

NAV and SYM are unchanged. ADJ fixes two vendor binding typos: `&mo TAB`
becomes `&kp TAB`, and `&bt LSHFT` becomes `&kp LSHFT`. The numeric layer
reference replaces `&mo NAV` for editor compatibility. MOU at index 4 is
unchanged, including all six thumb mouse buttons. Finger contact activates MOU;
its transparent letter positions inherit Graphite and home row mods. Layers 5
and 6 remain reserved. NAV still overrides the left home row with Bluetooth
controls, so home row mods are unavailable at those positions on NAV.

The following requested settings differ from the vendor main branch:

- `CONFIG_ZMK_POINTING_SMOOTH_SCROLLING=y` on the left enables the official
  HID resolution-multiplier feature. It does not make the device a native host
  multitouch trackpad. OS and application support affect the result.
- Removing `invert-scroll-y` from the TPS43 node reverses vertical two-finger
  scrolling relative to the vendor configuration. Horizontal scrolling and
  pointer orientation are unchanged.
- Adding `INPUT_TRANSFORM_Y_INVERT` to the existing thumb-layer scroll
  transformer reverses vertical hold-to-scroll relative to the vendor setting.
- Enabling the vendor's `TOUCAN_WIN_MODE` matches its win_mode branch. Pinch
  sends Ctrl+Minus and Ctrl+Equal for Linux and Windows. Three-finger swipes send
  Super+Tab, Super+Right, Super+D, and Super+Left for north, east, south, and west.
  Hyprland determines what those shortcuts do. No custom gesture logic is added.

Pointer scaling remains 100/100 and scroll scaling remains 1/20, both Toucan2
vendor values. The driver retains the vendor's tap, two-finger scroll, zoom,
three-finger swipe, filtering, and power-management options. Hardware tests
must confirm the desired direction and usable speed after flashing.

`config/toucan.json` adds row and column metadata for Nick's editor while
preserving all vendor coordinates. Build artifact names explicitly say Toucan2
because the vendor's internal shields still use `toucan_left` and `toucan_right`.

## Sources

- [Official Toucan2 firmware guide](https://docs.beekeeb.com/toucan2-keyboard/convert-toucan-to-toucan2#flashing-the-toucan2-firmware)
- [Toucan2 baseline](https://github.com/beekeeb/zmk-keyboard-toucan2/tree/6882c95)
- [Vendor Windows mode](https://github.com/beekeeb/zmk-keyboard-toucan2/tree/win_mode)
- [Azoteq driver documentation](https://github.com/beekeeb/zmk_driver_azoteq)
- [Azoteq configuration properties](https://github.com/beekeeb/zmk_driver_azoteq/blob/main/dts/bindings/input/azoteq,tps43-common.yaml)
- [ZMK smooth scrolling](https://zmk.dev/docs/config/pointing)
- [ZMK scroll transformer](https://zmk.dev/docs/keymaps/input-processors/transformer)

## Verification status

Firmware commit `bf7e758` passed the
[Toucan2 build](https://github.com/Dillpickleschmidt/zmk-keyboard-toucan2/actions/runs/34781919629)
on September 13, 2026. Both halves and the settings-reset target compiled.
That build predates the Escape/Ctrl and thumb changes described above.
The logs confirm `CONFIG_INPUT_TPS43=y` and `azoteq,tps43` on the right, plus
Studio, smooth scrolling, and status screen 2 on the left.

Static checks confirmed 42 bindings in each of the five active layers, unchanged
Graphite bindings, stock NAV/SYM/MOU bindings, and unchanged vendor matrix,
geometry, dependencies, display source, right-half config, and build workflow.
Nick's hosted editor loaded all layers and the Graphite layout without a layout
warning through Clipboard import.

This corrected build has not yet been flashed. A successful build alone cannot
confirm trackpad operation, direction, gesture shortcuts, or home row timing.
Those require the hardware tests in [the installation guide](setup.md).
