# Modifications

This file lists the changes in this repository compared with the versions they are based on.
It is included as required by **GPLv3, Section 5(a)**: modified versions must carry a prominent notice
that they were modified, along with the relevant date.

| Folder | Based on |
|---|---|
| `TDS_Macro_Night3HardMode/` | **Ultimate Macro 1.3.4a** ("New Era", [UltimateMacro/Ultimate-Macro-New-Era](https://github.com/UltimateMacro/Ultimate-Macro-New-Era)) with Night 3 support, originally by **Darksen** |
| `TDS_Macro/` | **Ultimate Macro 1.3.3** by **Darksen** ([DarksenDev/tds-macro](https://github.com/DarksenDev/tds-macro)) |

---

# `TDS_Macro_Night3HardMode/` (1.3.4a + Night 3)

Changes are in `Main.ahk` and `.gitignore`. Other files are unchanged from the version received.

## November 18, 2026

### 1. Drop the Beat cooldown setting (decimals allowed)
- **Before:** the cooldown was hard-coded to 28 seconds (`28000 / multiplier`) in `UseAbilitiesPass()`.
- **After:**
  - New **"Beat Cooldown (sec):"** field in the **Settings** tab (below the Hologram/Reposition keybinds), with a `?` help button. It is hidden on the *Advanced settings* page like the other keybind controls.
  - New `ParseBeatCooldown()` helper. It accepts `28`, `28.5`, or `28,5`. Empty, invalid, or below 1 falls back to **28**, and the maximum is 600.
  - The value is saved as `BeatCooldown` in the `[Options]` section of `%AppData%\Ultimate_Macro\Options\Settings.tds`.
  - `UseAbilitiesPass()` now uses `BeatCooldown * 1000 / multiplier`, so timescale is still applied.

### 2. DJ track switching: hover before click
- **Before:** `SetDJTrack()` called `Click(DJTrack.x, DJTrack.y)` directly. Roblox can ignore a click that lands the same instant the mouse arrives.
- **After:** the mouse moves to the button, waits about 150 ms, nudges 1 pixel and back, then calls the same `Click(DJTrack.x, DJTrack.y)`. The existing retry, cooldown, and deadline logic is unchanged.

### 3. Offline dependencies
- Removed `/lib/OCR.ahk` and `/lib/JSON.ahk` from `.gitignore`, so both files are committed with the repository. Without them the macro tries to download them on first launch. Both are MIT licensed.

### 4. Modification notice
- Added a comment at the top of `Main.ahk` saying this is a modified version. The original credit comment and the in-app Credits tab are **unchanged**.

### Not changed (already in 1.3.4a)
- Support Caravan no longer blocks Drop the Beat (same fix as `TDS_Macro/` item 2).
- Upgrade Delay is saved correctly from Settings.
- The Drop the Beat stun retry is limited to 3 attempts.

### Checks run
- AutoHotkey v2 `/validate` passes (installed and bundled `AutoHotkey64.exe`).
- Upstream `tests/test_source_contracts.py` passes, and the DJ/ability checks in `tests/test_reported_runtime_fixes.py` pass.
- `tests/test_reported_runtime_fixes.py` as a whole already failed before these changes, on an unrelated Night launch check (`CheckTheMapF`).

---

# `TDS_Macro/` (1.3.3)

All changes are in `TDS_Macro/Main.ahk`. Other files are unchanged from the original.

## November 2026

### 1. Drop the Beat cooldown setting
- **Before:** the Drop the Beat cooldown was hard-coded to 28 seconds in `UseAbilities()`.
- **After:**
  - New **"Beat Cooldown (sec):"** field in the **Settings** tab (below the Hologram/Reposition keybinds), with a `?` button that explains it.
  - The value is saved as `BeatCooldown` in the `[Options]` section of the settings file (`%AppData%\Ultimate_Macro\Options\Settings.tds`).
  - Default is **28**. Empty, invalid, or below 1 falls back to 28.
  - The timescale multiplier is still applied automatically.

### 2. Drop the Beat no longer blocked by Support Caravan
- **Before:** in `UseAbilities()`, when Auto Caravan was ON and no Commander had reached level 4, the code ran `return`. That stopped the whole function, so **Drop the Beat never ran**, even when DJ was already level 3. This showed up in strategies that turn on `autoCaravan` and upgrade Commander late (for example Fallen Down).
- **After:** that `return` was replaced with an `if (foundCommander) { ... }` block. Only the Caravan part is skipped. Drop the Beat (and the abilities after it) keep running.

### 3. DJ track switching (Red/Green) is more reliable
- **Before:** `SetDJTrack()` used `MouseClick` to move the mouse and click in the same instant. Roblox often ignores that click, so the button only looked hovered and was never pressed.
- **After:** the mouse moves to the button first, waits about 150 ms, nudges 1 pixel and back, and only then clicks.

### 4. Modification notice
- Added a short comment at the top of `Main.ahk` saying this is a modified version. The original credit comment from Darksen was left **unchanged**.

---

## Known issues (not yet changed)
- Drop the Beat is used right when DJ reaches level 3, even if the in-game ability may still be on cooldown.
- A press that fails because of cooldown (not stun) is still logged as successful.
- The "Successfully changed DJ track" log does not check whether the track actually changed.
- The Upgrade Delay field in Settings is not saved from the menu (the save code overwrites the field with the old value).
- The stun retry loop for Drop the Beat has no limit.
