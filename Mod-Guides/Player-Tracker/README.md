# APH Havoc Player Tracker DLC

Logs what players do to local JSON logs and Discord, with a System Manager tab to browse players, events and positions on a map. Works on any map.

Needs **APH Core** and CF. Server and clients load the same PBO.

## Logged (each kind can be switched on/off, to the log file, to Discord, and to its own webhook)
| Kind | When |
|---|---|
| connect / disconnect | joins and leaves, with position (and "dead" when leaving dead) |
| death | killer, weapon and distance; infected, animal, thirst, hunger, suicide |
| damage | hits from other players: zone, weapon, distance, damage, health left (infected / animals with `LogAIDamage`) |
| shots | shots summed per player and weapon over `ShotsWindowSeconds` ("fired 12 shots with M4-A1") |
| transfer | picked up / dropped, put into or taken from containers and bodies, items passed between players (moves inside one's own inventory are not logged) |
| uncon | fell unconscious / woke up |
| position | every `PositionEverySeconds` for every live player (also builds the route on the map) |

Shots, item moves and positions go to the log file only by default (they are many).

## Discord
Uses APH Core's `APH_CoreWebhookService`. Events are collected and posted as one embed per webhook every `DiscordEverySeconds` (at most two messages per webhook each time). **Watched groups**: Steam64 lists whose events also go to the group's webhook (Kinds empty = all; `AlsoNormalWebhook` false = only the group's).

## System Manager (End key) > Player Tracker
- **Players and map** (`playertracker.view`): every player seen, online first, kills / deaths / hits / shots; click one for their events and their route on the map.
- **Event log** (`playertracker.view`): filter by kind, search names / Steam64 / text, page back with Older.
- **Settings and webhooks** (`playertracker.settings`): everything above, watched groups, test webhook. Webhook links are only sent to staff with this feature.

## Files
- Settings (all maps): `$profile:aph_mods/APH_System_Manager/Config/PlayerTracker/Settings.json` (System > Reload configs > Player Tracker).
- Logs (per map): `$profile:aph_mods/APH_System_Manager/Data/PlayerTracker/<map>/<YYYY-MM-DD>.jsonl`, one JSON event per line; files older than `KeepDays` are deleted at start. Today's file is read back after a restart.

Classes start with `APH_PT_`; RPC namespace `APH_PlayerTracker`. Test: `runtest.ps1 -Mode aphPtTest`.