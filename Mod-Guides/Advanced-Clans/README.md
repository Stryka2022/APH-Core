# APH Havoc Advanced Clans

Map, markers, clans, chat, minimap and player stats in one menu for DayZ 1.30:

- a full-screen map with private, group and server markers
- clan groups with ranks, tags and live member positions
- live event markers for the convoy, heli crashes and airdrops, on the map and in 3D
- APH Chat with Global, Group, Moderator and Admin channels
- a minimap
- a **Stats** tab (body condition, vitals, latest hits) and an on-screen **hit monitor**
- the server's logo and link buttons (with APH Havoc Logo Addon)

---

## Requirements

| Mod | Needed |
|---|---|
| APH Core + Community Framework (CF) | Required, on the server and every client |
| APH Havoc Leaderboard | Optional. Fills kills, deaths, K/D, joins and play time on the Stats tab. |
| APH Havoc Convoy | Optional. Shows the running convoy on the map. |
| APH Havoc Logo Addon | Optional. Shows your logo and buttons on the Server tab. |
| APH Havoc Banking | Optional. Puts ATMs on the map. |

Avoid loading a second group/map system that uses the **M** key; rebind conflicting controls.

---

## Installation

1. Put `APH_Havoc_Advanced_Clans.pbo` and its `.bisign` with your APH mods, and its key in the server's `keys` folder.
2. Load it on the server and every client, after APH Core.
3. Start the server once. It creates `Config\Clans\` and `Data\Clans\` under `<profiles>\aph_mods\APH_System_Manager\`.
4. Give staff the `clans.admin` permission in APH Core's `Config\Permissions.json`. Owner, Admin and Moderator have it by default.

---

## Player keys (Options > Controls > APH Havoc Advanced Clans)

| Key | What it does |
|---|---|
| **M** | Open or close the map menu. **Esc** closes a popup first, then the menu. |
| **Insert** | New marker at the crosshair, or under the mouse on the map |
| **Delete** | Remove the nearest marker |
| **T** | Ping your group at the crosshair, or under the mouse on the map |
| **K** | Hide marker icons: traders, then events, then personal, then show all |
| **Y** | Switch chat channel. **Tab** also switches while typing. |
| **/** | Show or hide the minimap |
| **[** | Minimap zoom |
| **Shift+Insert** | Staff: new server marker |
| **Ctrl+H** | Move the minimap and other HUD parts (APH Core) |

On the map, **double-click** makes a marker, **click** a marker shows its details, and **right-click** edits it. Drag to pan and use the wheel to zoom.

---

## The map menu tabs

| Tab | What it shows |
|---|---|
| **Server** | Your logo, welcome text and link buttons |
| **Markers** | **Events** first (convoy, airdrops, heli crashes, nearest first, each with Locate and 2D/3D switches), then group and personal markers. Every row has a **Locate** button that centres the map on it. |
| **Group** | Make or join a clan, invites, members and ranks |
| **Stats** | The player's record (kills, deaths, K/D, hits, joins, play time), a front and back body coloured by each zone's health, open wounds, **what's wrong** (bleeding, broken leg, low blood, sickness ...), live vitals and the latest hits taken |
| **Settings** | Each player's own switches: event markers in 2D and 3D, the minimap, the hit monitor, and **Move** for the HUD editor |
| **Admin** | Staff only (`clans.admin`): server markers and groups |
| **HUD** | Opens the HUD editor |

**Hit monitor:** for 7 seconds after each hit, a small body on the left of the screen shows where you were hit and what's wrong. Each player can turn it off in Settings.

---

## Clans

- **Ranks:** Leader, Commander, Soldier, Recruit.
- **Inviting:** Soldier and up.
- **Kick, promote, demote:** Commander and up, only for members below you.
- **Handing over leadership:** the Leader promotes a Commander.
- **Leaving:** when the Leader leaves, the highest-ranked member takes over. An empty group is removed.

---

## APH Chat

Each chat line shows the clan tag, the name and the channel, for example `[APH] LEPRACHAUN: (Group) on my way`.

| Channel | Who can send and read it |
|---|---|
| Global | Everyone |
| Group | Your clan |
| Moderator | Moderator, Admin and Owner roles |
| Admin | Admin and Owner roles |

- **Game commands:** lines starting with `#` (such as `#login`) still go to the game.
- **Other chat interfaces:** compatibility varies depending on the chat mod installed.
- **Server log:** every line is written as `[APH Chat] [channel] [tag] name (steam64): text`.

---

## Server files (`<profiles>\aph_mods\APH_System_Manager\`)

| File | What it holds |
|---|---|
| `Config\Clans\ServerConfig.json` | Menu title, banner image, welcome and footer text, link buttons, groups on/off, group size, territory limit, tag and name lengths, marker limits, position update rate, invite expiry, and the switches `ShowConvoyMarkers`, `ShowHeliCrashMarkers`, `EnableChat`, `EnableMinimap`. Every player receives this file, so never put secrets in it. |
| `Config\Clans\ServerMarkers.json` | Server markers and zones: name, description, icon, colour, position, radius |
| `Data\Clans\Groups.json` | Clans, members, ranks and group markers. The server writes it after every change, so edit it only while the server is stopped. |
| `Data\Clans\Players\<steam64>.json` | Each player's own markers |
| `Data\Clans\Stats\<steam64>.json` | Lifetime hits per body zone |

- **Live reload:** **Reload config** in the Admin tab, or **System > Reload configs > Clans**, re-reads `ServerConfig.json` and `ServerMarkers.json`.
- **Marker icons:** a vanilla map symbol (`camp`, `tourism`, `monument`, `viewpoint`, `store`, `factory`, `police`, `firedep`, `station`, `transmitter`, `waterpump` ...), or a full `.paa` path.
- **Players' own settings** are saved on their PC in `<DayZ profile>\aph_mods\APH_System_Manager\Client\Clans.json`.

---

## Live events on the map

- **Convoy** (APH Havoc Convoy): moving while it drives, "under attack" while fought, then "Convoy loot" at the reward crate.
- **Heli crashes:** every crash site on the server, including ones loaded back after a restart.
- **Airdrops:** listed in the Markers tab's events.

To turn convoy or heli crash markers off for everyone, set `ShowConvoyMarkers` or `ShowHeliCrashMarkers` to `false`, then reload.

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| M opens another mod's map | Another map mod uses M. Rebind one of them. |
| Admin tab missing | The player lacks `clans.admin` in `Config\Permissions.json`. |
| Stats tab shows "-" for kills and play time | Load APH Havoc Leaderboard. |
| Chat is the vanilla chat | `EnableChat` is `false` in `ServerConfig.json`. |