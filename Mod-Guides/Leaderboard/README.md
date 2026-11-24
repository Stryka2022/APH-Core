# APH Havoc Leaderboard

A server leaderboard for DayZ 1.30. Every stat is counted on the server from vanilla game events: no additional gameplay expansion is required. Players open it with **U**.

---

## Requirements

- **APH Core** and **Community Framework (CF)**, on the server and every client.
- **APH Havoc Advanced Clans** (optional) shows the same stats on its Stats tab.

---

## Installation

1. Put `APH_Havoc_Leaderboard.pbo` and its `.bisign` with your APH mods, and its key in the server's `keys` folder.
2. Load it on the server and every client, after APH Core.
3. Start the server once. It creates `Config\Leaderboard\Settings.json` and `Data\Leaderboard\` under `<profiles>\aph_mods\APH_System_Manager\`.
4. Give staff `leaderboard.reset` in APH Core's `Config\Permissions.json`. Owner and Admin have it.

---

## Players

- **Open:** press **U** (Controls > APH Havoc Leaderboard).
- **Sort:** click a column header to sort, and click again to flip the order.
- **Top three:** they get gold, silver and bronze.
- **Your own stats:** your row is highlighted, and your stats and rank show under the table.

| Combat | Hunting | Shooting | Time |
|---|---|---|---|
| Player kills | Infected kills | Shots fired | Play time |
| Deaths, PvP deaths | Animal kills | Hits, headshots, HS % | Distance travelled |
| K/D | AI kills | Accuracy | Connections |
| Kill streak, best streak | | Longest shot | |

---

## Settings (`Config\Leaderboard\Settings.json`)

| Setting | What it does |
|---|---|
| `Title` | The leaderboard title |
| `Columns`, `SortColumn`, `SortDescending` | Which columns show, in what order, and the default sort |
| `MaxRows`, `HidePlayersWithNoStats` | Table size, and hiding empty players |
| `CountVehicleTravel` | Count distance driven, not only walked |
| `AIClassnameContains`, `ZombieClassnameContains`, `AnimalClassnameContains` | Classname filters for AI, infected and animal kills |
| `CountAIVictimsWithoutIdentityAsAI` | Treat players without an identity as AI |
| `SaveIntervalSeconds` | How often stats are saved (default 60, and on disconnect) |
| `EnableEventLog` | Write every event to `Data\Leaderboard\Events\<date>.tsv` |

Player stats are in `Data\Leaderboard\Players\<steam64>.json`. Player files from the older `$profile:EOE/PlayerDatabase` are copied in once on the first start, so stats carry over.

**Live reload:** **System > Reload configs > Leaderboard**.

---

## Admins

Open the APH shell, then **Leaderboard > Player stats** (`leaderboard.reset`).

- **Find:** search by name or Steam64.
- **Reset:** click **Reset** twice to clear one player's stats.