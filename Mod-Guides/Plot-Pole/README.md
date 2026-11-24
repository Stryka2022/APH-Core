# APH Havoc Plot Pole Addon

Paid upkeep for vanilla territory flags, for DayZ 1.30:

- Players pay to keep their flag raised, from the bank, cash or items.
- **Pack to kit** turns a flag pole back into a kit.
- An upkeep line appears on screen near a flag.
- Staff can wipe a whole territory.

---

## Requirements

| Mod | Needed |
|---|---|
| APH Core + Community Framework (CF) | Required, on the server and every client |
| APH Havoc Banking | Optional. Players pay from their bank account, and the addon can use the bank's currencies. |

---

## Installation

1. Put `APH_Havoc_PlotPole_Addon.pbo` and its `.bisign` with your APH mods, and its key in the server's `keys` folder.
2. Load it on the server and every client, after APH Core.
3. Start the server once. It creates `<profiles>\aph_mods\APH_System_Manager\Config\PlotPole\Settings.json`.
4. Give staff the permissions below in APH Core's `Config\Permissions.json`. Owner has them all.

---

## Players

Look at a flag pole and choose **Plot pole upkeep**. The menu shows:

- **Upkeep:** the percentage, a bar and the time left.
- **Payment:** where you pay from, and your balance.
- **Plans:** up to 4, for example +25%, +50% or refill to full.
  - A plan that would overfill only costs the share that fits.
  - A plan can also ask for items, such as nails and planks. It shows how many you carry against how many it needs, and takes them when you pay. A box of 70 nails counts as 70.
- **Pack to kit:** turns the pole back into a kit. By default this only works once nothing built or placed is left in its radius.

Free raising is blocked by default (`BlockFreeRaise`), so upkeep has to be paid.

---

## Admins

- **Admin wipe:** in the same menu, removes the flag and everything built in its radius. It asks first. Needs `plotpole.wipe`.
- **Plot Pole tile:** in the APH shell (`plotpole.admin`), it edits everything:
  - **Plans:** cost, upkeep added, and the items each plan needs, written as `Nails:20, WoodenPlank:10` (up to 6 per plan). A plan with cost 0 and items is paid in items only.
  - **Payment source:** **Bank, then cash**, **Bank only** or **Cash only**.
  - **Money:** the cash items and their values, or **Use the Banking system's currencies**.
  - **Options:** pack, HUD and wipe settings.

**Live reload:** the settings reload live, or from **System > Reload configs > Plot Pole**.

---

## Settings (`Config\PlotPole\Settings.json`)

| Setting | What it does |
|---|---|
| `BlockFreeRaise`, `BlockLower` | Stop raising without paying, and stop lowering |
| `Plans` | The upkeep plans |
| `PaymentSource`, `Currencies`, `UseBankCurrencies`, `CurrencySymbol` | How players pay |
| `PackEnabled`, `PackRequireEmpty`, `PackKitClassName` | Pack to kit |
| `HudEnabled`, `HudRange`, `HudShowTimeLeft` | The on-screen upkeep line |
| `AdminWipeEnabled`, `AdminWipeRadius`, `AdminWipeDeployables` | The admin wipe |

---

## Permissions

| Feature | Allows |
|---|---|
| `plotpole.admin` | The Plot Pole tile: plans, payment and options |
| `plotpole.wipe` | Admin wipe of a territory |

---

## Discord

The `PlotPole_ACTION` event posts through APH Core's shared webhooks (**System > Discord webhooks**) when upkeep is paid, a flag is packed or a territory is wiped.