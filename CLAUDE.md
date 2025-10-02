# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a ZMK (Zephyr Mechanical Keyboard) firmware configuration for the Piantor Pro BT keyboard, a 36-key split ergonomic keyboard with wireless Bluetooth connectivity and optional display support.

## Build and Development Commands

**Building firmware:**
- Firmware builds automatically via GitHub Actions on push/PR
- Manual builds trigger via GitHub Actions workflow dispatch
- Build configuration defined in `build.yaml`
- Firmware releases automatically created on main branch pushes with timestamp tags

**Local development:**
- `west init -l config app` - Initialize workspace using this repo as config application
- `west update` - Sync ZMK and dependencies from `config/west.yml`
- `west build -p always -b piantor_pro_bt_left config` - Build left half
- `west build -p always -b piantor_pro_bt_right config` - Build right half
- Check `build.yaml` for exact board/shield/snippet combinations used in CI

## Architecture and Key Files

**Core Configuration:**
- `config/piantor_pro_bt.keymap` - Main keymap definition with 6 layers and ZMK behavior configurations
- `config/piantor_pro_bt.json` - Physical layout definition with key positions and multiple layout variants
- `config/west.yml` - ZMK dependency manifest pointing to zmkfirmware/zmk main branch

**Build Configuration:**
- `build.yaml` - GitHub Actions matrix for building left/right halves with different configurations:
  - Studio-enabled builds with USB-UART RPC for ZMK Studio compatibility
  - Nice!View display shield support
  - Settings reset shield variants
- `.github/workflows/build.yml` - CI/CD pipeline using ZMK's shared workflow

**Keyboard Layout Structure:**
- **Layer 0 (QWERTY):** Base layer with home row mods (Shift/Ctrl/Alt on ASDF/JKL;)
- **Layer 1 (NUMBER):** Numbers, function keys, and basic navigation (accessed via Space hold)
- **Layer 2 (SYMBOL):** Symbols and arrow keys with shift modifiers (accessed via Enter hold)
- **Layer 3 (MEDIA):** Media controls, brightness, volume, page navigation (accessed via Backspace hold)
- **Layer 4 (MOUSE):** Mouse movement, clicks, and scroll wheel (accessed via Tab on Layer 1)
- **Layer 5 (BLUETOOTH):** Bluetooth profile management, system reset, bootloader access (accessed via ; on Layer 1)

**Key Technical Details:**
- Uses balanced mod-tap behavior with 200ms tapping term and retro-tap enabled
- Supports ZMK Studio for live keymap editing (studio-rpc-usb-uart snippet)
- Home row mods configured on ASDF (left) and JKL; (right) keys following GACS order (GUI/Alt/Ctrl/Shift)
- Layer access through layer-tap thumb keys (lt bindings) and momentary layer switches
- Custom "super" macro combines Ctrl+Alt+Cmd for app-switching workflows
- Mouse emulation support with movement and scroll wheel controls
- Physical layout defined in `piantor_pro_bt.json` with default 42-key and five_col variants

## Keymap Modification Guidelines

**Layer Organization:**
- Layer 0: Always the base/default typing layer
- Layers 1-3: Primary function layers (numbers, symbols, media)
- Layers 4-5: Specialized functions (mouse, bluetooth/system)

**Mod-Tap Configuration:**
- Current tapping-term-ms: 200ms with "balanced" flavor and retro-tap enabled (config/piantor_pro_bt.keymap:12-16)
- Home row mods follow GACS order (GUI/Alt/Ctrl/Shift) from pinky to index
- Thumb cluster uses layer-tap combinations for space efficiency
- When modifying timing, test thoroughly to avoid accidental modifier activation

**Display Names:**
- Each layer requires a display-name for Nice!View screen identification
- Keep names short and descriptive for screen real estate

**Testing Changes:**
- Run `west build` for both left and right halves after keymap edits
- Verify expected shield variants (studio, settings_reset) appear in build artifacts
- Test keymap changes with ZMK Studio using studio-enabled firmware before flashing
- Use settings_reset shield builds to clear EEPROM/stale Bluetooth profiles
- Verify both left and right halves build successfully in CI

**Coding Style:**
- Follow Zephyr DTS formatting: 4-space indentation, aligned angle-bracket arrays, trailing commas
- Layer nodes use snake_case (e.g., `number_layer`), display-name uses uppercase
- Keep ASCII art comments aligned with existing column guides
- Use descriptive behavior labels (`&mt`, `&lt`) over inline hex scancodes