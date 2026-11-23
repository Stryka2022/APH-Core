# APH Core

The shared engine behind every APH mod for DayZ 1.30. It runs:

- **System Manager:** permissions, Discord webhooks, live config reloads, the HUD layout editor, zones and the object placer.
- **Admin shell:** the End key menu that every APH mod adds its tile to.
- **Admin Tools code:** used when the APH Admin Tools PBO is loaded.
- **Storage:** storage, raiding, virtual storage, lights and the digital code locks for APH Storage and Container Homes.
- **Item scripts:** the Mag Station, Speed Loader, solar station, gun bench, auto gun racks and bed rest.

Load it on the **server and every client**. Every other APH mod requires it.

---

## Requirements

| Mod | Needed |
|---|---|
| DayZ 1.30 | Required |
| Community Framework (CF) | Required. It loads before APH Core. |

---

## Installation

1. Put `APH_Core.pbo` and its `.bisign` in your APH mod folder, for example `@APH Havoc Manager\addons`, or in its own `@APH_Core\addons`.
2. Put the signing key (`.bikey`) in the server's `keys` folder.
3. Load it on the server and on every client, straight after CF:
   ```
   -mod=@CF;@APH_Core;<other APH mods>;...
   ```
   Every APH mod lists `APH_Core` in its `requiredAddons`, so the game loads APH Core first even when they share one folder.
4. Start the server once. APH Core creates `<profiles>\aph_mods\APH_System_Manager\`.
5. Add yourself as Owner (see **Permissions** below).

---

## Server folders

Every APH system keeps its files in one place, `<profiles>\aph_mods\APH_System_Manager\`:

```
APH_System_Manager\
├── Config\              settings you edit (or change in game)
│   ├── Permissions.json     who can use what (shared by every APH mod)
│   ├── HudLayout.json       the server's default HUD positions
│   ├── System\Webhooks.json shared Discord webhooks
│   ├── Zones\Zones.json     safe zones and no-build zones
│   └── <Mod>\...            one folder per APH mod (Banking, Clans, Garage, Killfeed ...)
├── Data\                files the server writes (accounts, groups, stats ...); edit only while the server is stopped
└── Client\              on each player's PC: their own settings
```

The storage system keeps its own folder, `<profiles>\aph_mods\aph_storage\` (see **Storage** below).

---

## Permissions

`Config\Permissions.json` decides who can open the admin shell and which tools they get. It is created on the first start.

- **Members:** a Steam64 ID with a **Role**, plus optional per-person **Allow** and **Deny** lists.
- **Roles:**
  - **Owner** has every feature (`*`), including editing permissions.
  - **Admin** has all of Admin Tools plus live reloads, the HUD default, clan admin, delete-at-crosshair and leaderboard reset. Features from other mods, such as `banking.admin` or `plotpole.wipe`, must be added to the role.
  - **Moderator** has player actions, teleport, ESP, announcements and clan admin.
  - You can edit the roles or add new ones.
- **Feature names** look like `banking.admin` or `admintools.teleport`. `admintools.*` means every Admin Tools feature. `FeatureList` in the file lists every feature from every loaded APH mod, and is rewritten on each start.
- **First Owner:** put your 17-digit Steam64 under `Members` with the role `Owner`. Until one is filled in, nobody can open the tools, and the server log says `[APH] Permissions.json: loaded, but no Steam64 is filled in`.
- **No restart needed:** the server rereads the file every few seconds. In game, **System > Permissions** edits it.

| Feature | What it allows |
|---|---|
| `system.permissions` | Edit permissions |
| `system.reload` | Reload configs live |
| `system.webhooks` | Edit the shared Discord webhooks |
| `system.hudlayout` | Save the server's default HUD layout |
| `zones.edit` / `zones.build` | Draw zones / build inside no-build zones |
| `world.delete_target` | Delete the object at the crosshair |
| `admintools.*` | Admin Tools (see the APH Admin Tools README) |

Each APH mod adds its own features, listed in its README.

---

## The admin shell

1. Press **Home** to switch your admin keys on. The message that appears names the key that opens the menu.
2. Press **End** to open the shell. Both keys are under Options > Controls > APH.
   - **Older builds:** before 2026-11-16 the default was Delete, which is also the Clans "Remove marker" key.
   - **Players who already changed their keys** keep their own binds, so rebind it to End there if needed.

Each APH mod adds a tile here. The built-in tiles are:

- **Admin Tools**
- **System:** permissions, webhooks, reloads and HUD layout.
- **Zones**
- **Theme:** the shell's colours, saved on your own PC.

You only see the tiles your permissions allow.

---

## Discord webhooks

Most APH mods post to Discord through one shared list of webhooks: **System > Discord webhooks** in game, or `Config\System\Webhooks.json`.

- **Several webhooks:** add one per channel. Each has an `Url`, an on/off switch, and the `Events` it sends (`"*"` sends every event).
- **SteamFilter:** limits a webhook to events involving those Steam64 IDs.
- **Templates:** each event's message is a Discord message template with tokens such as `{Killer}`. Overrides change a template for one webhook only.
- **Rate limit:** messages are queued and sent within Discord's rate limit.

| Mod | Events |
|---|---|
| APH Havoc Killfeed | `Kill_PVP`, `Kill_HEADSHOT`, `Kill_MELEE`, `Kill_EXPLOSION`, `Kill_ROADKILL`, `Kill_HELI`, `Kill_AI`, `Kill_SUICIDE`, `Kill_FALL`, `Kill_NATURAL`, `Kill_INFECTED`, `Kill_ANIMAL`, `Streak_MILESTONE` |
| APH Havoc Banking | `Bank_TRANSFER`, `Bank_ADMIN` |
| APH Havoc Plot Pole Addon | `PlotPole_ACTION` |
| APH Havoc Virtual Garage | `Garage_IMPOUND` |

Some parts have their **own** webhook settings in their own files:

- **Admin Tools:** `Config\AdminTools\Webhooks.json`.
- **Raids:** `aph_storage\aph_storage.json`.
- **Code locks:** `aph_storage\aph_storage_digital_lock.json`.
- **Mag Station:** `aph_mag_repair_station\aph_mag_repair_station.json`.
- **APH Havoc Convoy:** `APH_Convoy\Discord.json`.

---

## Live reloads

**System > Reload configs** re-reads a mod's files and sends the new settings to every player, with no restart. Each APH mod with settings adds its entry.

---

## HUD layout editor

Players move the APH HUD parts themselves: the Clans minimap, the killfeed and the kill notification.

- **Open it:** press **Ctrl+H** (Controls > APH, "Move HUD"), or **Move** in the map's Settings tab. Drag the red boxes.
- **Save:** each player's layout is saved on their own PC.
- **Reset to default:** goes back to the server's positions.
- **Save as server default:** shows for `system.hudlayout`. It writes `Config\HudLayout.json` and sends it to everyone who hasn't moved their own parts.

---

## Zones: safe zones and no-build zones

Draw zones in **System Manager > Zones** (`zones.edit`).

1. **Draw the outline:** pick **Draw safe zone** or **Draw no-build zone**. Click each corner, or hold the left mouse button and drag to draw freehand. Right-click takes back a point. Click the first point or **Finish shape** to close it.
2. **Adjust:** drag a corner to move it, drag a side's middle dot to add a corner, right-click a corner to remove it, or drag inside to move the whole zone.
3. **Name and save:** choose whether it shows on players' maps, then **Save for everyone**.

What each kind does:

- **Safe zones** are shaded green on the map. Players get a note when they walk in or out.
- **No-build zones** are outlined red. Nobody can build or place anything inside them, except staff with `zones.build`.

Zones are saved in `Config\Zones\Zones.json`.

---

## Placing objects (ATMs, parking meters ...)

The placer shows a preview at your crosshair inside a hologram box: **green** when the spot is clear, **red** when it overlaps something (a red spot only places with Shift held).

- **Rotate:** the mouse wheel turns it 5° a notch (Shift 1°, Ctrl 0.1°). Q / E turn it while held (25° a second, Shift 6, Ctrl 1). **G** turns it 90°.
- **Snap:** **F** lines it up with the wall or object you aim at: square to its sides, its back flush against the wall, standing on the floor.
- **Tilt and lean:** Alt + wheel, Alt + Q / E.
- **Height:** Up / Down. **R** resets.
- **Finish:** **Enter** or left click places it, **Backspace** cancels.

The wheel steps are their own keys in Controls ("APH Placing: turn left / right").

---

## Welcome screen and server rules

Players see a welcome window in the middle of the screen once they have loaded in (after the spawn menu). It has a **WELCOME** tab and a **RULES** tab, a **Discord** button (opens the invite and copies it), and an accept button.

- **Open it again:** **SERVER RULES** in the Esc menu, or the **Insert** key (Controls > APH HUD, "APH Server rules and welcome screen").
- **Edit it:** **System Manager > Welcome & Rules** (`system.welcome`). Each line of the welcome text and of the rules is its own box. **Save for everyone** writes the file and sends it to every player. **Preview** shows it to you without saving. **Show to everyone** opens it on every player's screen now.
- **New joiners accept the rules first:** with **Must accept before playing** (default on) a new player gets the window as soon as they load in, before the APH spawn menu, with no Close button or X; Esc does not close it. Once they press the accept button it never comes back.
- **New joiners only** (default on): players who already had a character on the server when this was added never get it. Raise **Rules version** after changing the rules to make everyone accept again.
- **Show it:** *Until they accept the rules* (default), *First join only*, or *Every time they join*.
- **Files:** `Config\Welcome\Welcome.json` (everything above, by hand if you like; then **Reload configs**), and `Data\Welcome\Seen.json` (who has seen it and which rules version they accepted).

---

## Storage

APH Core runs everything in APH Storage and Container Homes:

- open and close
- digital code locks (and the classic code lock and vanilla combination lock)
- raiding with tools or breaching charges, on a schedule
- virtual storage (contents are stored away while closed)
- lights
- gear swap on mannequins and lockers
- kit crafting and dismantling
- bed rest

The settings are in `<profiles>\aph_mods\aph_storage\`:

| File | What it holds |
|---|---|
| `aph_storage.json` | Code lock support, raiding (tools, damage, time, breaching charges, HMC4), proxies, auto close, whitelists (ammo boxes, fridges, medical cabinets), bed rest, kit crafting and dismantling, virtual storage, logs, the raid webhook |
| `aph_raid_schedule.json` | When raiding is allowed |
| `aph_storage_raid_target_control.json` | Which storage can be raided |
| `aph_storage_digital_lock.json` | Code lock admins, remembered access, debug log, code lock webhooks |
| `aph_gun_bench.json`, `five_tier_locker.json` | The gun bench and five-tier locker |
| `raid_logs.log`, `interaction_logs.log`, `startup.log` | Logs |

- **Restart after editing:** these files are read at start-up.
- **Reload key:** admins can also press **Numpad 2** to reload the storage config.
- **Updates:** missing settings from an update are added to your files on the next start.

### Digital code locks

- **Fitting a lock:** fit one to any APH storage with a code lock slot. The keypad opens to set the code.
- **Correct code:** unlocks the storage and opens it.
- **Lock:** locks it and closes every door and lid.
- **Wrong codes:** repeated wrong codes lock players out for a while.
- **Remembered access:** whoever sets the code, or enters it correctly, is remembered and gets straight in next time. A player is forgotten after `RememberDays` (5) offline. Changing the code or removing the lock forgets everyone.
- **Admin removal:** Steam64 IDs in `AdminSteam64IDs` get **Admin remove digital lock**, which works even on a locked lock.

### Gear swap

**Swap / Store Gear** works on every APH storage with clothing slots: mannequins, lockers, the gear cabinet and the five-tier locker.

- **Swapping:** each slot swaps in one step. Clashing items go through a hidden holder, so nothing is dropped.
- **Refused swaps:** if a slot can't swap, both items stay put and you're told why. The server log line `[APH GEAR] refused` names the item and the check that refused it.

---

## Languages

APH text follows each player's game language. Every APH mod ships a `stringtable.csv` with the 13 DayZ languages. Text an admin types (names, labels) shows as typed.

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| `Can't compile "World" script module!` | Read the line under it in the crash log. It names the file and line, and is usually a load-order problem or an outdated APH mod. Every APH mod must come from the same release as APH Core. |
| Nobody can open the admin shell | Put your Steam64 in `Config\Permissions.json` as `Owner`. Press **Home**, then **End**. If End does nothing, check "Open / close admin menu" under Controls > APH. |
| A tile is missing in the shell | Your role lacks that mod's permission, or the mod isn't loaded. |
| No Discord posts | Turn the webhook on, check its `Url`, and make sure its `Events` list includes the event, or `*`. |
| Storage renders grey or white | The APH Storage PBO was built without its textures. Rebuild it with every `.paa` packed. |
| A remembered player still gets the keypad | They were away longer than `RememberDays`, or the code changed. |
| Changed an `aph_storage` file and nothing happened | Restart the server, or press Numpad 2 as an admin. |

---

**APH Core** by MISFITNO1 / APH Mods
