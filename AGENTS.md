# Repository Guidelines

## Project Structure & Module Organization
This repo holds a ZMK configuration for the Piantor Pro BT split board. Active firmware sources live under `config/`: `piantor_pro_bt.keymap` defines all six layers and behaviors, `piantor_pro_bt.json` maps the physical layout, and `west.yml` pins ZMK upstream. Board definitions stay in `boards/arm/piantor_pro_bt/` (currently used for overrides), while CI recipes sit in `build.yaml`. Reference screenshots and documentation live in `screenshots/` and `README.md`.

## Build, Test, and Development Commands
Firmware builds in GitHub Actions by default, but you can reproduce them locally once you have the ZMK toolchain. Typical flow:
- `west init -l config app` : create a workspace that uses this repo as the config application.
- `west update` : sync ZMK and dependent modules pinned in `config/west.yml`.
- `west build -p always -b piantor_pro_bt_left config` : build the left half (repeat with `piantor_pro_bt_right`).
Check `build.yaml` for the exact board/shield/snippet combos mirrored by CI.

## Coding Style & Naming Conventions
Follow Zephyr-style DTS formatting: four-space indentation, aligned angle-bracket arrays, and trailing commas to ease diffs. Layer nodes use snake_case names such as `number_layer`, but human-facing `display-name` strings stay concise and uppercase for Nice!View readability. Keep comment art aligned with the existing column guides and prefer descriptive behavior labels (`&mt`, `&lt`) over inline hex scancodes.

## Testing Guidelines
Run `west build` for both halves after meaningful keymap edits and ensure the build artifacts carry the expected `studio` or `settings_reset` variants when toggled via `build.yaml`. Validate mod-tap timing and layer logic in ZMK Studio using the studio-enabled firmware before flashing both sides. When altering Bluetooth behavior, perform a settings reset build (`shield: settings_reset`) to clear stale profiles.

## Commit & Pull Request Guidelines
Commit history mixes short updates with conventional-commit prefixes (`feat(keymap):`, `docs:`); follow that pattern and scope changes narrowly. Describe functional outcomes (e.g., “adjust tapping term for home-row mods”) rather than file lists, and keep formatting-only edits separate. Pull requests should link any tracked issues, note which firmware variants were built, and, when UI-affecting, attach updated photos or Studio screenshots.

## Configuration Tips
Balance thumb-layer assignments so the `lt` bindings remain symmetric, and avoid renaming `display-name` entries without updating the Nice!View layer order. Use the `studio_unlock` binding sparingly—remove it if you distribute public firmware. Document any custom shield additions by extending `build.yaml` so the CI matrix keeps producing complete artifact sets.
