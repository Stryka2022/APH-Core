# APH Havoc Spawns

A spawn menu for every fresh character, on first join and every respawn, for DayZ 1.30. Players pick one of:

- a spawn point on a map
- one of their own respawn beds
- a random spawn

Each choice can have its own cooldown, cost and loadout.

---

## Requirements

| Mod | Needed |
|---|---|
| APH Core + Community Framework (CF) | Required, on the server and every client |
| APH Item Statics | Optional. Gives spawn loadouts and loot pools. |
| APH Havoc Banking | Optional. Charges for spawns and beds. |
| APH Storage | Optional. Its beds work as respawn beds. |

---

## Installation

1. Put `APH_Havoc_Spawns.pbo` and its `.bisign` with your APH mods, and its key in the server's `keys` folder.
2. Load it on the server and every client, after APH Core.
3. Start the server once. It finds the map and creates `Config\Spawns\Settings.json` and `Config\Spawns\Maps\<map>.json` under `<profiles>\aph_mods\APH_System_Manager\`.
   - **chernarusplus**, **enoch** (Livonia), **sakhal** and **nasdara** come with spawn points made from each map's vanilla spawns. Sakhal and Nasdara areas are named by compass direction, so rename them in game.
   - Any other map starts empty and keeps the vanilla spawn until you add points.
4. Give staff `spawns.admin` in APH Core's `Config\Permissions.json`. Owner has it.

---

## Players

- **Spawn menu:** lists your beds and the spawn points on a map, each with its cooldown and cost.
  - Pick one and press **Spawn here**, or press **Random**.
  - Nobody can hurt you while the menu is open.
  - If you don't choose within `ChoiceSeconds`, you get a random spawn.
- **Respawn beds:** look at a placed APH bed and use **Respawn bed (set / clear)**.
- **Death blocks:**
  - Dying near a spawn point blocks that point for a while.
  - Dying near your bed puts the bed on cooldown.

---

## Admins (APH shell > **Spawns**, permission `spawns.admin`)

- **Spawn points:**
  - **Add point here** makes a point where you stand, and **+Pos** adds your position to a point.
  - Each point has on/off, cooldown, radius, cost and loadouts, plus **Go** (teleport) and **Delete**.
- **Respawn beds:** bed classnames, beds per player, cooldown, uses, cost and bed loadouts.
- **Settings:** menu time, random spawn, protection while choosing, default loadout, stats on spawn, and the death block.

**Live reload:** **System > Reload configs > Spawns**.

---

## Settings (`Config\Spawns\Settings.json`)

| Setting | What it does |
|---|---|
| `SpawnMenuOnEveryJoin` | `1` = also show the menu when a player joins with their existing character (they move to the chosen spawn with their own gear) |
| `ChoiceSeconds`, `AllowRandom`, `RandomIgnoresCooldowns` | The menu timer and random spawns |
| `ProtectWhileChoosing` | No damage while the menu is open |
| `DefaultLoadout` | Loadout when a spawn has none |
| `SetStatsOnSpawn`, `Water`, `Energy`, `Health`, `Blood` | Starting stats |
| `DeathBlockRadius`, `DeathBlockSeconds` | The death block |
| `BedsEnabled`, `BedClasses`, `BedLimit`, `ReplaceOldest` | Respawn beds |
| `BedCooldownSeconds`, `BedKilledRadius`, `BedMaxUses`, `BedCost` | Cooldown after spawning on the bed or dying within `BedKilledRadius` of it, uses (`-1` = unlimited), cost |
| `BedLoadouts`, `BedStripStarterGear` | What you get when you spawn on a bed |

| File | What it holds |
|---|---|
| `Config\Spawns\Maps\<map>.json` | That map's spawn points |
| `Data\Spawns\Beds.json` | Claimed beds |
| `Data\Spawns\Cooldowns.json` | Cooldowns (they survive restarts) |

---

## Loadouts and loot pools

These are made in the APH shell's **Loadouts** tile, which comes from APH Item Statics.

- **Spawn loadouts** and **admin kits** can be named on a spawn point or bed.
- **Loot pools** are reusable item groups, for example "Primary weapon" with several guns. With **Give one random item** on, one is picked.
- When a spawn has no loadout of its own, the weighted spawn loadouts are used.