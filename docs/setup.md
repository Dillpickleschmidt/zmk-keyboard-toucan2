# Inspect and install Graphite on Toucan2

Use this repository only for the Toucan2 with its rectangular Azoteq TPS43
trackpad. The earlier `zmk-keyboard-toucan` fork targets different hardware.

## Inspect the keymap

1. Open [Nick Coutsos's Keymap Editor](https://nickcoutsos.github.io/keymap-editor/).
2. Select GitHub and sign in.
3. Grant access to `Dillpickleschmidt/zmk-keyboard-toucan2`.
4. Select that repository, branch `main`, and `config/toucan.keymap`.
5. Inspect BASE, NAV, SYM, ADJ, MOU, and GAME.
6. Inspect `hml`, `hmr`, `quote_pair`, `comma_pair`, `minus_pair`, and `slash_pair` in Behaviors.

For a local preview, use FileSystem and select this repository's `config`
directory in a Chromium browser. For Clipboard import, choose Custom, import
`config/toucan.json`, paste `config/toucan.keymap`, and select Set Keymap.

The editor shows keys and behaviors, not the trackpad driver configuration.
MOU is the vendor's touch-activated layer, with mouse buttons on the thumbs.

## Build firmware

1. Open this repository's Build ZMK firmware workflow in GitHub Actions.
2. Run the workflow on `main` if no build started automatically.
3. Wait for all jobs to succeed.
4. Download and extract the `firmware` artifact.

Use `toucan2_graphite_left.uf2` and `toucan2_graphite_right.uf2`.
Do not flash the settings-reset artifact for a normal update.

## Replace the incorrect original-Toucan firmware

The [official Toucan2 firmware guide](https://docs.beekeeb.com/toucan2-keyboard/convert-toucan-to-toucan2#flashing-the-toucan2-firmware)
identifies the correct vendor repository. Flash both halves with this build.

1. Connect the left half by USB data cable.
2. Double-press its physical RST button near USB-C.
3. Verify that the XIAO bootloader drive appears.
4. Copy `toucan2_graphite_left.uf2` to the drive.
5. Wait for the drive to disappear and the keyboard to restart.
6. Connect the right half by USB data cable.
7. Double-press its RST button.
8. Copy `toucan2_graphite_right.uf2` to its XIAO drive.
9. Wait for the right half to restart.
10. Reconnect the left half by USB and keep the right half powered.

Studio's saved keymap can override compiled layers, including MOU at index 4.
Record any Studio edits you want to retain before restoring the compiled map.

11. Connect the left half in [ZMK Studio](https://zmk.studio/).
12. With your fingers off the trackpad, hold the middle left thumb key and press the original QWERTY Z-position key.
13. Select Restore Stock Settings to discard saved overrides and load this firmware's Graphite keymap.

Here, "stock" means this custom Toucan2 build, not factory QWERTY. See
[ZMK's keymap persistence warning](https://zmk.dev/docs/features/studio#keymap-changes).
Studio edits do not update GitHub or Nick's editor. Keep permanent changes in
this repository. Bluetooth hosts may need re-pairing after HID descriptor changes.

## Test the corrected hardware configuration

1. Use a US host keyboard layout without a second Graphite remapper for the Toucan2.
2. Test letters on both halves in a blank document.
3. Move one finger on the trackpad without holding any keys.
4. Test single-tap, two-finger-tap, and press-and-hold dragging.
5. Test two-finger vertical and horizontal scrolling in a browser.
6. Compare vertical scroll direction with the laptop trackpad.
7. Touch the trackpad and test the MOU thumb mouse buttons.
8. Lift your finger before testing NAV and SYM thumb-layer access.
9. Test the Graphite Shift pairs and opposite-hand home row shortcuts.
10. Test pinch-to-zoom in a browser.

Three-finger swipes send the vendor's Windows-mode shortcuts. Their effects on
Hyprland depend on your existing bindings. No Hyprland settings are changed here.
Scroll direction and smoothness still require a physical test on each host.

## Install and test the gaming update

If both halves already run the corrected Toucan2 firmware, flash only the left
half with the new `toucan2_graphite_left.uf2`. Do not use the settings-reset file.

1. Press the bottom-right key to toggle GAME on.
2. Check that the display shows GAME.
3. Test Left Shift at the middle key of the second left-hand column, with the top and bottom keys inactive.
4. Tap the far-left middle key to test Escape.
5. Touch the trackpad and press the right inner thumb to test Space.
6. Test J and L at their physical QWERTY positions, with left-click at K between them.
7. Hold that key while moving the pointer to test dragging.
8. Press the left inner thumb to test right-click.
9. Touch the trackpad and check that both middle thumbs do nothing.
10. Press the bottom-right key again to return to BASE.
11. Check that the normal layout, NAV, SYM, and touch-activated thumb mouse buttons return.

The [gaming reference](graphite.md#gaming-layer) lists the bindings and layer behavior.
