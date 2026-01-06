# ZMK Config - Microdix Keyboard

This is a ZMK firmware configuration for a custom **Microdix** split mechanical keyboard optimized for French and programming.

## Hardware Overview

- **Keyboard**: Microdix - 36-key ergonomic split layout
- **MCU**: Nice!Nano v2 (Bluetooth enabled)
- **Split Configuration**: Right half is the central controller

## Project Structure

```
boards/shields/microdix/   # Hardware definitions (matrix, pins, overlays)
config/
├── microdix.keymap        # Main keymap (12 layers)
├── microdix.conf          # Firmware settings (BLE, debounce)
├── west.yml               # Dependencies (ZMK forks, modules)
└── include/               # Modular configuration
    ├── settings.dtsi      # Global behaviors
    ├── homerow_mod.dtsi   # Home row mods
    ├── combos.dtsi        # Key combos
    ├── macros.dtsi        # Custom macros
    └── ...
```

## Layer Architecture

| Layer | Name | Purpose |
|-------|------|---------|
| 0 | BASE | Main QWERTY-inspired layout |
| 1 | NAV | Navigation (arrows, home, end, page up/down) |
| 2 | MEDIA | Media controls, Bluetooth, bootloader |
| 3 | NUM | Number pad |
| 4 | SYM | Symbols |
| 5 | FUN | Function keys |
| 6 | SFT_SYM | Shifted symbols |
| 7+ | DIAC_* | French diacritical marks |

## Key Configuration Files

- **Pin assignments**: `boards/shields/microdix/microdix.dtsi` and overlays
- **Debounce tuning**: `config/microdix.conf` (CONFIG_ZMK_KSCAN_DEBOUNCE_*)
- **Bluetooth settings**: `config/microdix.conf`

## Building

Firmware is built via GitHub Actions. Push to trigger a build, then download artifacts.

```bash
# Local build (requires ZMK toolchain)
west build -b nice_nano_v2 -- -DSHIELD=microdix_left
west build -b nice_nano_v2 -- -DSHIELD=microdix_right
```

## Coding Guidelines for This Project

### DeviceTree (.dtsi/.overlay/.keymap)
- Use meaningful names for behaviors (e.g., `hml` for home-mod-left)
- Keep position references using zmk-helpers macros (LT0, RT0, etc.)
- Comment complex behaviors and combos
- Maintain ASCII art layout comments in keymap for visual reference

### When Modifying Layers
- Update the layer index defines if adding/removing layers
- Test both halves after matrix changes
- Check combo positions match actual key positions

### When Changing Hardware Pins
- Update both left and right overlays if needed
- Remember right side uses different pins for some rows (col-offset)
- Test debounce settings after pin changes

### Dependencies
- Main ZMK fork: `infused-kim/zmk` (urob improvements)
- Additional modules defined in `config/west.yml`
