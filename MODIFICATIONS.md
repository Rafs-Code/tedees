# Modifications

This file lists the changes in this repository compared with the versions they are based on.
It is included as required by **GPLv3, Section 5(a)**: modified versions must carry a prominent notice
that they were modified, along with the relevant date.

| Folder | Based on |
|---|---|
| `SolarRequiem/` | **"SolarRequiem"**: Ultimate Macro 1.3.4a ("New Era") with Night 3 and Night 4 support, shared by another player, originally by **Darksen** |
| `TDS_Macro_Night3HardMode/` | **Ultimate Macro 1.3.4a** ("New Era", [UltimateMacro/Ultimate-Macro-New-Era](https://github.com/UltimateMacro/Ultimate-Macro-New-Era)) with Night 3 support, originally by **Darksen** |
| `TDS_Macro/` | **Ultimate Macro 1.3.3** by **Darksen** ([DarksenDev/tds-macro](https://github.com/DarksenDev/tds-macro)) |

---

# `SolarRequiem/` (1.3.4a + Night 3 + Night 4)

Changes are in `Main.ahk` and `.gitignore`. Other files are unchanged from the version received.
The received copy was nested in a `SolarRequiem - Copy/` subfolder. Its contents were moved up to `SolarRequiem/`.

## November 19, 2026

### What SolarRequiem already added (not my work)
Compared with `TDS_Macro_Night3HardMode/`, SolarRequiem adds Night 4:
- New `N4` mode in the Record tab difficulty list (strategy: `difficulty=N4`).
- New `IsNightMode()` helper. Every place that checked `difficulty = "Night"` now treats `Night` and `N4` the same way (lobby, Ready, loadout, cutscene skip, Return To Lobby, loss handling).
- Night 4 card detection with `Resources/Night4.png` and OCR text "Night 4", plus variant images `Night4Easy.png` / `Night4Hard.png`.

### 1. Drop the Beat cooldown setting (decimals allowed)
Same as `TDS_Macro_Night3HardMode/` item 1: **"Beat Cooldown (sec):"** field in Settings, `ParseBeatCooldown()` (accepts `28`, `28.5`, `28,5`; default 28; max 600), saved as `BeatCooldown`, and used by `UseAbilitiesPass()` instead of `28000`.

### 2. DJ track switching: hover before click
Same as `TDS_Macro_Night3HardMode/` item 2.

### 3. Bug fixes in the SolarRequiem changes
- **Arcade card image path:** `TryClickArcadeTarget()` built the path as `... ".png" ".png"` (for example `Pizza Party.png.png`). That file never exists, so Arcade/Trial image detection always failed and fell back to OCR. Now uses a single `.png`.
- **Night variant state:** `SelectNightVariant()` dropped the line `eventDifficulty := targetName`. Restored, same as the Night 3 version.
- **Indentation:** several Night blocks in `RunStrategy()`, `PlayStrategy()`, `TryClickDifficultyTarget()`, `SelectNightVariant()`, and `JoinGame()` had broken indentation (and tabs mixed with spaces). Re-indented with the original comments restored. Whitespace and comments only, no logic change.

### 4. Offline dependencies and notice
- Same as `TDS_Macro_Night3HardMode/` items 3 and 4: `lib/OCR.ahk` and `lib/JSON.ahk` are committed, and `Main.ahk` has a modified-version notice. The original credit comment and the in-app Credits tab are unchanged.

## November 20, 2026

### 5. Hacker clone retry wait: set in Settings
- **Before:** every failed clone attempt (hologram cooldown, not enough cash, Hacker stunned, cannot place here) waited a fixed 4650 ms in real time, about 9.3 seconds of game time at 2x.
- **After:**
  - New row in the **Settings** tab (below Beat Cooldown): **"Clone Retry OFF:"** and **"2x:"**, plus a `?` help button.
  - **Timescale OFF:** waits the OFF value (default **4.65 s**, same as before).
  - **Timescale 2x:** waits the 2x value (default **1.25 s**).
  - **Timescale 1.5x:** no setting, calculated automatically as OFF value / 1.5 (default 3.10 s).
  - Decimals allowed (`1.25` or `1,25`), range 0.25–30 s. Saved as `CloneRetryOff` and `CloneRetry2x` in `[Options]` of the settings file.
  - `CloneTower()` uses the new `GetCloneRetryMs()`, and the log shows the actual wait (for example `Retrying again in 1.25 seconds...`).
- The UI waits inside the clone (hover scan, 0.3 s after the Hologram key, and so on) are unchanged, because Roblox draws its UI in real time. Each attempt therefore takes the retry wait plus about 1.5–3 s of mouse movement.

### 6. Night end: keep watching until Return To Lobby
- **Before:** after the last strategy step, `PlayStrategy()` called `SkipNightCutscene(120000)` once, then `ClickNightReturnToLobby()` (30 s). If Return To Lobby didn't appear, the macro went idle for the rest of the match. In one Night 4 run it held a skip button 7 s after the last step, gave up on Return To Lobby 30 s later, and the real end cutscene after the Umbra boss was never skipped.
- **After:** new `FinishNightMatch(900000)` loops for up to 15 minutes. Each pass: if Return To Lobby is visible, click it (hover first) and check that it disappeared; if HOLD TO SKIP is visible, hold it. It keeps going after mid-match cutscenes until Return To Lobby is clicked. `ClickNightReturnToLobby()` is no longer called.

### 7. Night skip hold: hover first and verify
- **Before:** `SkipNightCutscene()` pressed the button with `Click(x, y, "Down")` (move and press in the same instant), held a fixed 5.5 s, and reported success without checking.
- **After:** new `HoldNightSkipButton()`:
  - Hovers first (move, wait 150 ms, nudge 1 px), then presses.
  - Holds at least 5.5 s, then checks every 0.5 s and releases as soon as the button is gone (max 8 s).
  - After releasing, checks again. If the button is still visible, it holds again (max 3 attempts).
- Used both for the intro skip at match start and for the end-of-match watcher. At match start a finished hold still counts as done, like before, so it never causes a reload.
- Abilities (Drop the Beat, and so on) are **not** paused during the hold.

### Checks run
- AutoHotkey v2 `/validate` passes (installed and bundled `AutoHotkey64.exe`).
- Upstream `tests/test_source_contracts.py` and `tests/lint_strategies.py` pass, and the DJ/ability checks in `tests/test_reported_runtime_fixes.py` pass.
- `tests/test_reported_runtime_fixes.py` as a whole fails on the same Night launch check (`CheckTheMapF`) as the Night 3 version did before any of my changes.
- Not tested in-game yet.

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
