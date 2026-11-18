# Modifications

This file lists the changes in this repository compared with the original **Ultimate Macro** by **Darksen**
([DarksenDev/tds-macro](https://github.com/DarksenDev/tds-macro)), version **1.3.3**.

It is included as required by **GPLv3, Section 5(a)**: modified versions must carry a prominent notice
that they were modified, along with the relevant date.

All changes are in `TDS_Macro/Main.ahk`. Other files are unchanged from the original.

---

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
