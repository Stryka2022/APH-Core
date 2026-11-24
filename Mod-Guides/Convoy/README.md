# APH Havoc Convoy

An armed AI convoy event for DayZ 1.30:

- Three vehicles crewed by Expansion AI guards drive a road route across the map.
- Players ambush it and kill every guard to unlock a care package marked with purple smoke.
- The convoy shows live on the APH Clans map and in 3D.
- It announces itself on Discord.
- Staff build routes in game with the convoy editor.

---

## Requirements

| Mod | Needed |
|---|---|
| APH Core | Required, on the server and every client |
| DayZ Expansion AI | Required, with the Expansion mods it needs |
| APH Havoc Advanced Clans | Optional. Shows the convoy on the map, in the events list and in 3D. |
| LBmaster Core | Optional. Only needed to use LB loot presets in the crate. |

Load exactly **one** current APH Core. A second or older copy gives `Can't find variable 'APH_ConvoyProfile'`.

---

## Installation

1. Put `APH_Havoc_Convoy.pbo` and its `.bisign` with your APH mods, and its key in the server's `keys` folder.
2. Load it on the server and every client, after APH Core and Expansion AI.
3. Start the server once. It creates `<profiles>\aph_mods\APH_Convoy\` and the guard loadout `<profiles>\ExpansionMod\Loadouts\APH_Convoy_Rifleman.json`.
4. Give route builders the `convoy.edit` permission in APH Core's `Config\Permissions.json`. Owner has it.

---

## Files (`<profiles>\aph_mods\APH_Convoy\`)

| File | What it is |
|---|---|
| `ConvoyEvent.json` | The event: start rules, timing, routes and stops, vehicles, AI, loot and notices |
| `Crate.json` | Care package loot: items, and optional LB presets |
| `Discord.json` | Discord settings |
| `Presets\` | Routes exported from the editor |
| `Status.json` | Written every 5 seconds. The Clans map reads it. Don't edit it. |
| `Command.json` | Start and stop requests from the editor. Don't edit it. |

- **Live edits:** changes are picked up within about 5 seconds, with no restart.
- **Updates:** existing files are never overwritten. `Discord.json` is re-saved on start, so new settings appear in it.

---

## When it runs (`ConvoyEvent.json`)

| Setting | Default | What it does |
|---|---|---|
| `AutoStart` | `true` | Start on its own |
| `MinPlayersOnline` | `1` | Only start while at least this many players are online. It never starts on an empty server. |
| `StartDelayAfterPlayersSeconds` | `1200` | Starts this long after enough players are online (20 minutes). The wait restarts if the server empties. |
| `InitialDelaySeconds` | `60` | Wait after server start |
| `CooldownSeconds` | `3600` | Wait between convoys |
| `StopWhenEmptySeconds` | `300` | A running convoy stands down (no reward) after the server has been empty this long. `0` keeps it running. |
| `MaxEventSeconds` | | The longest a convoy may run |

---

## Convoy editor (F8)

Press **F8** (Controls > APH Havoc Convoy editor), or open the **Convoy** tile in the APH shell. It needs `convoy.edit`. In the editor you can:

- draw and edit routes and their named stops
- set up patrols
- start or stop the convoy now
- export routes to `Presets\`

---

## Care package (`Crate.json`)

- **`Items`:** the loot list.
- **`LBPresets`:** optional LBmaster presets, each with its own `Chance` (0 to 1).
- **`MaxLBPresets`:** caps how many presets are picked (`0` = no cap).
- **`LBPresetsOnly: true`:** fills the crate from presets only.

Without LBmaster loaded, the presets are skipped and a line goes to the log.

---

## Discord (`Discord.json`)

```json
{
  "Enabled": true,
  "WebhookURL": "https://discord.com/api/webhooks/....",
  "Username": "APH Havoc Convoy",
  "MentionText": "",
  "OnConvoyStart": true,
  "OnLocationChange": true,
  "OnArrival": true,
  "OnAmbush": true,
  "OnConvoyDefeated": true,
  "OnConvoyEnded": false,
  "IncludeRouteStops": true,
  "IncludeCoordinates": true,
  "IncludePlayerCount": true
}
```

| Post | When |
|---|---|
| **Convoy rolling out** | It sets off: route, start, destination, length, every stop, vehicles and guard count |
| **On the move** | It passes each named stop, with the next stop |
| **Arrived** | It reaches the end of its route |
| **Under attack** | It is ambushed, with the first attacker's name |
| **Defeated** | Every guard is dead |
| **Ended** | It stands down |

`MentionText` goes above the message, for example `@everyone` or `<@&ROLE_ID>`.

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| `Can't find variable 'APH_ConvoyProfile'` | An old or second APH Core is loaded. Keep only the current one. |
| The convoy never starts | Fewer than `MinPlayersOnline` players are online, or `StartDelayAfterPlayersSeconds` hasn't passed yet. |
| No guards | Expansion AI isn't loaded, or the loadout file is missing. Delete it and restart to get it back. |
| No convoy on the map | Load APH Havoc Advanced Clans, and check `ShowConvoyMarkers` in its `ServerConfig.json`. |