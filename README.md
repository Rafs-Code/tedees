# Ultimate Macro for TDS — Modified Version

> **This is NOT my original project.**
> This repository is a **modified version** of **Ultimate Macro** for Tower Defense Simulator.
> - Original creator: **Darksen** ([DarksenDev/tds-macro](https://github.com/DarksenDev/tds-macro))
> - Newer version (1.3.4a "New Era") and Night 3 support: the **Ultimate Macro: New Era** team and contributors ([UltimateMacro/Ultimate-Macro-New-Era](https://github.com/UltimateMacro/Ultimate-Macro-New-Era))
>
> All credit for the macro goes to them. I only made a few small fixes for my own use. The full list is in [MODIFICATIONS.md](MODIFICATIONS.md).

---

## About this macro

**Ultimate Macro** is a macro (automation tool) for the Roblox game **Tower Defense Simulator (TDS)**, written in **AutoHotkey v2**. It replays a *strategy* (a `.strat` file) that contains tower placements, upgrades, and ability use, then repeats the match automatically.

Main features (from the original macro):

- **Record & play strategies**: record tower placement, upgrades, and actions, then replay them.
- **Night 3 support** (Easy/Hard): Night card and variant selection, loadout, Ready, cutscene skip, and Return To Lobby (in `TDS_Macro_Night3HardMode`).
- **Automatic game handling**: detects when a game ends, checks the map, and clicks *Restart* / *Play Again*.
- **Strategy rotation**, **auto-equip towers**, **Timescale** (1.5x / 2x) support, and **playtime rewards** collection.
- **Auto abilities**: Call of Arms, Support Caravan, Drop the Beat, auto skip wave.
- **Discord webhook / bot**, **VIP server** support, multi-path towers, and more.

## Which folder should I use?

| Folder | Version | Use it for |
|---|---|---|
| **`TDS_Macro_Night3HardMode/`** | 1.3.4a (New Era) + Night 3 + my changes | **Recommended.** Newest version, supports Night 3 and normal modes. |
| `TDS_Macro/` | 1.3.3 (Darksen) + my changes | Older version, kept for reference. No Night support. |

## What is different in this version

A short summary (details and dates are in [MODIFICATIONS.md](MODIFICATIONS.md)):

1. **The Drop the Beat cooldown can be set from Settings.** New *Beat Cooldown (sec)* field in the Settings tab. Default is 28, and decimals are allowed (for example `28.5`, or `28,5`).
2. **More reliable DJ track switching (Red/Green).** The mouse hovers over the button briefly before clicking, so Roblox registers the click.
3. **Drop the Beat is no longer blocked by Support Caravan.** This fix is in `TDS_Macro/`. The New Era version already includes the same fix.

## Folder structure

```
.
├── TDS_Macro_Night3HardMode/   # Recommended: 1.3.4a + Night 3 (modified)
│   ├── Main.ahk                # Main script, run this
│   ├── lib/                    # Helper libraries (Gdip, OCR, JSON, ImageSearch, etc.)
│   ├── Resources/              # Images for screen detection, maps, sample strats
│   ├── submacros/              # Watchdog, updater, auto CoA/spin/consumable
│   ├── tests/, tools/          # Upstream validation scripts
│   └── LICENSE                 # GPLv3 (upstream)
├── TDS_Macro/                  # Older 1.3.3 version (modified)
├── stratEksternal/             # Strategies made by other people (see Credits)
├── LICENSE                     # GPLv3 (same as the original project)
├── MODIFICATIONS.md            # List of changes from the originals
└── README.md                   # This file
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
3. Run `TDS_Macro_Night3HardMode/Main.ahk`.
4. Set your keybinds and **Beat Cooldown** in the **Settings** tab, then click **Save all settings**.
5. Pick or record a strategy, then start it.

**Ability settings live in the strategy file.** Auto Call of Arms only runs if the `.strat` file has `autoChain=ON` (and Support Caravan needs `autoCaravan=ON`, Drop the Beat needs `autoDropTheBeat=ON`):

```ini
[Settings]
autoChain=ON
autoCaravan=ON
autoDropTheBeat=ON
```

**Important notes:**

- **If an "Update Available" popup appears, click _No_.** The built-in updater downloads the official version and **overwrites** the changes in this version.
- Settings and recordings are stored in `%AppData%\Ultimate_Macro\`, **not** in this folder. On a new computer you need to set them up again and copy your `.strat` files over.

## Credits

### Original macro
- **Darksen** — original creator of **Ultimate Macro**.
  - GitHub: [DarksenDev/tds-macro](https://github.com/DarksenDev/tds-macro)
  - YouTube: [@darksenn](https://www.youtube.com/@darksenn)
  - Discord: [discord.gg/DQnc2JDJtr](https://discord.gg/DQnc2JDJtr)
  - If you like this macro, please support the original author: [donate to Darksen](https://www.donationalerts.com/r/darksen1).

### Ultimate Macro: New Era (version 1.3.4a, used as the base of `TDS_Macro_Night3HardMode`)
- GitHub: [UltimateMacro/Ultimate-Macro-New-Era](https://github.com/UltimateMacro/Ultimate-Macro-New-Era)
- Team, as listed in the macro's Credits tab:
  - Original Creator: Darksen
  - Lead Developer: pizzaroles24
  - Developers: ziadod, kronoxxv, banana.dev
  - Development Contributor: 4riff
  - QA: hetzel401, tristanm1ce, frostzzz
- **Night 3 support:** I got this version from another player. The files don't say who added Night 3, so the credit goes to the New Era team and contributors above. If you added Night 3 support, please open an *issue* so your name can be added.

### Libraries used (included in `lib/`)
- **AutoHotkey** — [autohotkey.com](https://www.autohotkey.com/)
- **Gdip_All (AHK v2)** — port by [buliasz/AHKv2-Gdip](https://github.com/buliasz/AHKv2-Gdip), originally by tic and the AHK community
- **Gdip_ImageSearch** — MasterFocus
- **OCR.ahk** — [Descolada/OCR](https://github.com/Descolada/OCR) (MIT), based on the UWP OCR function by malcev
- **JSON.ahk** — thqby & HotKeyIt ([thqby/ahk2_lib](https://github.com/thqby/ahk2_lib), MIT)
- **Roblox.ahk / Discord.ahk** — SP
- **OpenCV** (`opencv_world500.dll`) — [opencv.org](https://opencv.org/)

All of these belong to their respective authors and follow each one's license. See `TDS_Macro_Night3HardMode/DEPENDENCIES.md` for pinned versions.

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
- The original author also asked: *"Modifications are welcome, however stealing credit is not."* This version respects that. The original credit comments at the top of each `Main.ahk` and the in-app Credits tab are kept unchanged.

## Disclaimer

- This project is **not affiliated** with Roblox, Paradoxum Games (TDS developer), Darksen, or the Ultimate Macro: New Era team.
- Using a macro may violate the rules of Roblox or the game. **Use it at your own risk.**
- No warranty of any kind (see the GPLv3 license).
