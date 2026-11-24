# APH Havoc PvP and PvE DLC

PvP and PvE zones for any map, drawn in the APH System Manager, shown on players' maps, with a PvP / PvE badge on screen and notes when players walk in or out.

Needs **APH Core** (CfgPatches `APH_Core`) and CF. Server and clients load the same PBO.

## What it does
- **Zones**: draw any outline on the map (corners or freehand). Each zone is PvP or PvE, with
  - Priority (where zones overlap, the highest wins),
  - Leave delay (PvP zones: seconds a player stays PvP after walking out, stops zone hopping in a fight),
  - own enter / leave notes, colour, shown on maps or not, notes on or off.
- **Outside zones** the map's default mode applies (PvP by default).
- **Hits between players**: blocked unless the rule says PvP. `BothMustBePvP` (default on): both players must be in PvP. Off: only the victim's mode counts. Blocked: bullets, melee, vehicles driven by another player (`BlockVehicleHits`), and in PvE explosions nobody can be blamed for (`BlockExplosionsInPvE`). Infected, animals and AI are never affected. The attacker gets a short note (`TellBlockedAttacker`).
- **The server decides each player's mode** once a second and tells the player when it changes: HUD badge (mode, zone, PvP countdown) and a note on entry / exit.
- **Maps**: zones are drawn on whatever map menu is open (vanilla map, APH Havoc Advanced Clans map, ...); with Clans the zone names also show as map markers.

## System Manager (End key) > PvP / PvE (feature `pvpzones.edit`)
- **PvP and PvE zones**: draw, edit, colour and set the rules of each zone; Save for all sends them to the server and every player at once.
- **Rules and HUD**: default mode, hit rules, notes, map display, badge on / off and its position.
- **Players now**: every online player's mode and zone, on a list and a map; blocked hits since start.

## Files (one per map, so every map works)
`$profile:aph_mods/APH_System_Manager/Config/PvPZones/<map>/Zones.json`, for example `.../PvPZones/chernarusplus/Zones.json`, `.../nasdara/Zones.json`. Made on first start; System > Reload configs > "PvP / PvE zones" reads it again without a restart.

## Notes
- Hits are judged in `PlayerBase.EEOnDamageCalculated` on the server; other mods that also override it keep working (the chain calls super).
- Script classes all start with `APH_PZ_`; RPC namespace `APH_PvPZones`.
- Test: `_APH_DevTools/runtest.ps1 -Mode aphPzTest` (harness `APH_TrkTest.c`, server half `APH_TrkTest_World.c`).