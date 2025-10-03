# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a forked ZMK (Zephyr Mechanical Keyboard) firmware configuration for the Piantor Pro BT keyboard, a 42-key split ergonomic keyboard with wireless Bluetooth connectivity and Nice!View Gem display support.

**Fork customizations:**
- Nice!View Gem display integration via M165437's nice-view-gem project
- Custom 4-layer layout optimized for programming and media control
- "OIL" text macro combo (positions 41+36 triggers `nvim -c OIL`)

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
- `config/piantor_pro_bt.keymap` - Main keymap with 4 layers and custom OIL macro combo
- `config/piantor_pro_bt.json` - Physical layout definition with default 42-key and five_col 30-key variants
- `config/west.yml` - Dependencies: zmkfirmware/zmk (main) + M165437/nice-view-gem (main)

**Build Configuration:**
- `build.yaml` - GitHub Actions matrix building left/right halves with:
  - Shields: `nice_view_adapter nice_view_gem` for Nice!View Gem display
  - Snippet: `studio-rpc-usb-uart` for ZMK Studio support
  - Both: settings_reset variants for clearing EEPROM/Bluetooth profiles
  - ZMK Studio enabled on all builds (`-DCONFIG_ZMK_STUDIO=y`)
- `.github/workflows/build.yml` - CI/CD pipeline using ZMK's shared workflow

**Keyboard Layout Structure:**
- **Layer 0 (Default):** QWERTY base with TAB/CTRL/Shift modifiers, thumbs: mo(2)/SPACE/SHIFT | BKSP/ENTER/ESC
- **Layer 1 (Lower):** Numbers 1-9/0 on home/top rows, navigation arrows (left/down/right), page up/down, GUI on thumb
- **Layer 2 (Raise):** Symbols (!@#$%^&*), paired brackets (()[]{}<>), arithmetic operators, question mark
- **Layer 3 (Adjust):** Bluetooth controls (BT_NXT/PRV), RGB toggle/effects, backlight controls, media keys (play/prev/next), studio_unlock

**Key Technical Details:**
- Nice!View Gem display via M165437's nice-view-gem project for improved visualization
- ZMK Studio enabled for live keymap editing over USB-UART RPC
- OIL macro: Combo on thumb cluster (36+41) outputs "nvim -c OIL" string
- Physical layouts: 42-key default_layout (6×3+6 thumbs) and 30-key five_col_layout (5×3+6 thumbs)
- Layer access: Momentary layer switches (mo bindings) on outer columns and thumb keys

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

## cc-sessions
Include: @CLAUDE.sessions.md
