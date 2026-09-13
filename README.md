# ZMK config for beekeeb Toucan2 Keyboard

This personal fork is for Dylan's **Toucan2 with the rectangular Azoteq TPS43
trackpad**, not the original Toucan with a circular Cirque trackpad.

- [Inspect, flash, and test this configuration](docs/setup.md)
- [Graphite layout, home row mods, and Toucan2 settings](docs/graphite.md)

The firmware artifacts are named `toucan2_graphite_left.uf2` and
`toucan2_graphite_right.uf2`. Internal shield and keymap names remain `toucan`
because Beekeeb's Toucan2 repository uses those names.

[The beekeeb Toucan2 Keyboard](https://beekeeb.com/introducing-toucan2/) is a wireless split 42-key column‑stagger keyboard that a display and a trackpad, with an aggressive stagger on the pinky columns.

# Customizations

- **Keymap**: [config/toucan.keymap](config/toucan.keymap)
- **General configs**: [boards/shields/toucan/toucan_left.conf](boards/shields/toucan/toucan_left.conf) and [boards/shields/toucan/toucan_right.conf](boards/shields/toucan/toucan_right.conf)
- **Swipe shortcuts**: the `swipe_button_mapper` node in [boards/shields/toucan/toucan.dtsi](boards/shields/toucan/toucan.dtsi)
- **Invert scroll / trackpad settings**: the `tps43_trackpad` node in [boards/shields/toucan/toucan_right.overlay](boards/shields/toucan/toucan_right.overlay)

# License

The code in this repo is available under the MIT license.

The included shield nice_view_gem is modified from https://github.com/M165437/nice-view-gem licensed under the MIT License.

The linked trackpad module is based on https://github.com/geeksville/zmk_driver_azoteq

ZMK code snippets are taken from the ZMK documentation under the MIT license.

The embedded font QuinqueFive is designed by GGBotNet, licensed under under the SIL Open Font License, Version 1.1.
