# APH Item Statics

Full stats for any item on the inspect screen, plus the **Loadouts** editor that APH Havoc Spawns uses, for DayZ 1.30. Needs only APH Core and CF: no additional gameplay expansion required.

---

## Requirements

- **APH Core** and **Community Framework (CF)**, on the server and every client.

---

## Installation

1. Put `APH_Item_Statics.pbo` and its `.bisign` with your APH mods, and its key in the server's `keys` folder.
2. Load it on the server and every client, after APH Core.
3. Start the server once. It creates `Config\ItemStatics\Settings.json` and `Config\Loadouts\Loadouts.json` under `<profiles>\aph_mods\APH_System_Manager\`.
4. Give staff the Loadouts permissions below in APH Core's `Config\Permissions.json`. Owner has them all.

---

## The inspect screen

Inspect an item as normal (right-click > Inspect). The APH layout opens.

- **Left:** the item preview (drag to rotate, wheel to zoom), name, description, and condition, weight, liquid and temperature chips.
- **Right:** the APH stats panel:
  - **Health bar:** coloured by condition, with exact hitpoints from the server.
  - **Stats tab:** condition, weight and size, quantity, food and water values, magazine rounds, fire rates, spread, zeroing, ammo damage and speed, armour, melee and power values.
  - **Compatible tab:** magazines, ammo, the projectile it fires, and every item that fits each attachment slot.
  - **Config tab:** the item's raw config. Rows with `>` open the next level.
  - **Search box:** filters the open tab. Selecting a row shows its details at the bottom.

The panel scales with the screen. It keeps the vanilla widget names, to support compatibility with other inspect-screen extensions.

### Settings (`Config\ItemStatics\Settings.json`)

| Setting | Default | What it does |
|---|---|---|
| `Enabled` | true | `false` = the vanilla inspect screen |
| `ShowCompatibilityTab` | true | Show the Compatible tab |
| `ShowConfigTab` | true | Show the Config tab |
| `ConfigTabRequiresPermission` | false | `true` = only staff with `itemstatics.config` see the Config tab |
| `ShowExactHealth` | true | Ask the server for exact hitpoints (only for items you carry or stand next to) |
| `MaxListRows` | 400 | Rows shown before the list asks you to search |

**Live reload:** **System > Reload configs > Item Statics**.

---

## Loadouts (APH shell > **Loadouts**)

| Page | What it holds |
|---|---|
| **Spawn loadouts** | Gear given on spawn, each with a weight. APH Havoc Spawns uses these, and points or beds can name one. |
| **Loot pool** | Reusable item groups, for example "Primary weapon" with several guns. With **Give one random item** on, one of them is picked. Loadouts and kits include pools by name. |
| **Admin kits** | Ready-made kits staff spawn on themselves, on the ground, at the cursor, or on a player. |

Everything is saved in `Config\Loadouts\Loadouts.json`.

---

## Permissions

| Feature | Allows |
|---|---|
| `loadouts.edit` | Edit spawn loadouts, loot pools and admin kits |
| `loadouts.spawn` | Spawn admin kits on yourself, the ground, the cursor or players |
| `itemstatics.config` | The Config tab, when `ConfigTabRequiresPermission` is on |