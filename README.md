# Ultimate Macro for TDS — Modified Version

> **This is NOT my original project.**
> This repository is a **modified version** of **Ultimate Macro** by **Darksen**
> ([DarksenDev/tds-macro](https://github.com/DarksenDev/tds-macro)).
> All credit for the original macro goes to Darksen and its contributors.
> I only made a few small fixes for my own use. The full list is in [MODIFICATIONS.md](MODIFICATIONS.md).

---

## About this macro

**Ultimate Macro** is a macro (automation tool) for the Roblox game **Tower Defense Simulator (TDS)**, written in **AutoHotkey v2**. It replays a *strategy* (a `.strat` file) that contains tower placements, upgrades, and ability use, then repeats the match automatically.

Main features (from the original macro):

- **Record & play strategies**: record tower placement, upgrades, and actions, then replay them.
- **Community strategies**: download `.strat` files made by other players.
- **Automatic game handling**: detects when a game ends, checks the map, and clicks *Restart* / *Play Again*.
- **Strategy rotation**: switches between two strategies after a set number of runs or minutes.
- **Auto-equip towers**, **Timescale** (1.5x / 2x) support, and **playtime rewards** collection.
- **Auto abilities**: Call of Arms, Support Caravan, Drop the Beat, auto skip wave.
- **Discord webhook**, **VIP server** support, multi-path towers, and more.

## What is different in this version

A short summary (details and dates are in [MODIFICATIONS.md](MODIFICATIONS.md)):

1. **The Drop the Beat cooldown can be set from Settings.** There is a new *Beat Cooldown (sec)* field in the Settings tab (default 28) instead of a hard-coded value.
2. **Drop the Beat is no longer blocked by Support Caravan.** Previously, if Auto Caravan was on but no Commander had reached level 4 yet, Drop the Beat never ran either.
3. **More reliable DJ track switching (Red/Green).** The mouse now hovers over the button briefly before clicking, so Roblox registers the click.

## Folder structure

```
.
├── TDS_Macro/          # Macro source (modified version)
│   ├── Main.ahk        # Main script, run this
│   ├── lib/            # Helper libraries (Gdip, OCR, ImageSearch, etc.)
│   ├── Resources/      # Images for screen detection, maps, sample strats
│   ├── submacros/      # Extra scripts (auto CoA, auto spin, watchdog, etc.)
│   ├── LICENSE         # Original license (GPLv3)
│   └── README.md       # Original README by Darksen (unchanged)
├── stratEksternal/     # Strategies made by other people (see Credits)
├── LICENSE             # GPLv3 (same as the original project)
├── MODIFICATIONS.md    # List of changes from the original
└── README.md           # This file
```

## Requirements

- **Windows 10/11**
- **[AutoHotkey v2.0](https://www.autohotkey.com/)** (not v1.1), 64-bit
- Screen resolution **1920x1080** recommended, Windows scaling **100%** (required)
- **English (US)** Windows language pack installed (needed for OCR)
- Roblox/TDS settings: UI Scale **Large**, Screen Shake **off**, Prefer Vertical Upgrades **on**, no custom fonts

## How to use

1. Download/clone this repository, then **extract** it if it is a ZIP. The macro refuses to run from inside a ZIP.
2. Install **AutoHotkey v2**.
3. Run `TDS_Macro/Main.ahk`.
4. Set your keybinds in the **Settings** tab, then click **Save all settings**.
5. Pick or record a strategy, then start it.

**Important notes:**

- **If an "Update Available" popup appears, click _No_.** The built-in updater downloads the original version from Darksen's GitHub and **overwrites** the changes in this version.
- Settings and recordings are stored in `%AppData%\Ultimate_Macro\`, **not** in this folder. On a new computer you need to set them up again and copy your `.strat` files over.

## Credits

### Original macro
- **Darksen** — creator of **Ultimate Macro**.
  - GitHub: [DarksenDev/tds-macro](https://github.com/DarksenDev/tds-macro)
  - YouTube: [@darksenn](https://www.youtube.com/@darksenn)
  - Discord: [discord.gg/DQnc2JDJtr](https://discord.gg/DQnc2JDJtr)
  - If you like this macro, please support the original author: [donate to Darksen](https://www.donationalerts.com/r/darksen1).
- Everyone who helped Darksen build this macro.

### Libraries used (included in `TDS_Macro/lib`)
- **AutoHotkey** — [autohotkey.com](https://www.autohotkey.com/)
- **Gdip_All (AHK v2)** — port by [buliasz/AHKv2-Gdip](https://github.com/buliasz/AHKv2-Gdip), originally by tic and the AHK community
- **Gdip_ImageSearch** — MasterFocus
- **JSON.ahk** — thqby & HotKeyIt
- **OCR.ahk** — based on the UWP OCR function by malcev
- **Roblox.ahk / Discord.ahk** — SP
- **OpenCV** (`opencv_world500.dll`) — [opencv.org](https://opencv.org/)

All of these belong to their respective authors and follow each one's license.

### Strategies in `stratEksternal/`
These strategies were **not made by me**:
- **Fallen Down** (Cataclysm, Fallen; V4, V5-STABLE, and variants) — by **menz.7**. The `FallenDown_byMenzModified` file is a modified version of their strategy.
- `PikaBoo1.5.1N.strat`, `solo frost ss.strat`, and the `strategy-lab` file — I don't know who made them. They were downloaded from the community.

If you made any of these strategies and want your name added or the file removed, please open an *issue* on this repository.

### Modifications
- Small changes in this version were made by the owner of this repository, with help from Claude (Anthropic). See [MODIFICATIONS.md](MODIFICATIONS.md).

## License

This project follows the license of the original project: the **GNU General Public License v3.0** (GPLv3). The full text is in [LICENSE](LICENSE).

That means:
- This macro stays **free and open source**. Anyone may use, study, modify, and share it.
- Modified versions (including this one) **must stay GPLv3**, **keep the original credit**, and **clearly state what was changed**.
- The original author also asked: *"Modifications are welcome, however stealing credit is not."* This version respects that. Darksen's credit at the top of `Main.ahk` is kept unchanged.

## Disclaimer

- This project is **not affiliated** with Roblox, Paradoxum Games (TDS developer), or Darksen.
- Using a macro may violate the rules of Roblox or the game. **Use it at your own risk.**
- No warranty of any kind (see the GPLv3 license).
