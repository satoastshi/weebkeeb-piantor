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
- Use ZMK's west build system (requires ZMK development environment setup)
- No local build commands configured in this repo - builds happen in CI/CD

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
- **Layer 1 (NUMBER):** Numbers, function keys, and basic navigation
- **Layer 2 (SYMBOL):** Symbols and arrow keys with shift modifiers
- **Layer 3 (MEDIA):** Media controls, brightness, volume, page navigation
- **Layer 4 (MOUSE):** Mouse movement, clicks, and scroll wheel
- **Layer 5 (BLUETOOTH):** Bluetooth profile management, system reset, bootloader access

**Key Technical Details:**
- Uses balanced mod-tap behavior with 250ms tapping term
- Supports ZMK Studio for live keymap editing (studio-rpc-usb-uart snippet)
- Home row mods configured on ASDF (left) and JKL; (right) keys
- Layer access through mod-tap thumb keys and momentary layer switches
- Mouse emulation support with movement and scroll wheel controls

## Keymap Modification Guidelines

**Layer Organization:**
- Layer 0: Always the base/default typing layer
- Layers 1-3: Primary function layers (numbers, symbols, media)
- Layers 4-5: Specialized functions (mouse, bluetooth/system)

**Mod-Tap Configuration:**
- Current tapping-term-ms: 250ms with "balanced" flavor
- Home row mods follow GACS order (GUI/Alt/Ctrl/Shift) from pinky to index
- Thumb cluster uses layer-tap combinations for space efficiency

**Display Names:**
- Each layer requires a display-name for Nice!View screen identification
- Keep names short and descriptive for screen real estate

**Testing Changes:**
- Test keymap changes with ZMK Studio if using studio-enabled builds
- Use settings_reset shield builds to clear EEPROM if needed
- Verify both left and right halves build successfully in CI