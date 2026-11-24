# APH Havoc Admin Tracker DLC

Logs what staff do to local JSON logs and Discord, with a System Manager tab to review it. Works on any map.

Needs **APH Core** and CF. Server and clients load the same PBO.

## Logged
- **APH Admin Tools**: every action Admin Tools carries out (it writes an audit line for each): item and vehicle spawns, teleports (self, players, map, locations), radius / crosshair deletes, god mode, unlimited ammo, heal / kill / freeze / kick / ban, free cam on/off and body moves, spectate, inventory tools, time / weather / restarts, types.xml edits, creature spawns, announcements. Each entry keeps the action, the target, where the staff member stood and the spot they aimed at.
- **Refused**: Admin Tools actions tried without permission (log file only by default).
- **System Manager**: permission edits (add / change / remove staff), config reloads, safe / no-build zones saved, webhooks changed, HUD layout saved, welcome screen saved or shown, Admin Tracker settings changed.

Categories follow the Admin Tools permission groups: Teleport, Spawns, Deletes, Player actions, ESP / free cam, Server, Types XML, Creatures, Announcements, Access, Inventory, System Manager, Refused. Each category: on/off, log file, Discord, own webhook.

## Discord
Batched every `DiscordEverySeconds` through APH Core's `APH_CoreWebhookService`. **Staff groups**: Steam64 lists whose actions also go to the group's webhook (Categories empty = all; `AlsoNormalWebhook` false = only the group's). `IgnoredActions` (Admin Tools action names) and `IgnoredSteamIDs` are never logged.

## System Manager (End key) > Admin Tracker
- **Staff actions** (`admintracker.view`): filter by category, search, page back with Older.
- **Staff and map** (`admintracker.view`): each staff member's totals; their actions on the map (a dot per action, a line to the spot aimed at).
- **Settings and webhooks** (`admintracker.settings`).

## Files
- Settings: `$profile:aph_mods/APH_System_Manager/Config/AdminTracker/Settings.json` (System > Reload configs > Admin Tracker).
- Logs (per map): `$profile:aph_mods/APH_System_Manager/Data/AdminTracker/<map>/<YYYY-MM-DD>.jsonl`; the last 7 days are read back at start; files older than `KeepDays` (60) are deleted.

## How it hooks in
`modded class` on APH Core: `APH_Service` (Handle / Audit), `APH_SM_Access` (reload, save / remove member), `APH_ZonesServer`, `APH_SM_Webhooks`, `APH_HudLayoutServer`, `APH_WelcomeServer`. Nothing in APH Core was changed. Classes start with `APH_ATk_`; RPC namespace `APH_AdminTracker`. Test: `runtest.ps1 -Mode aphAtkTest`.