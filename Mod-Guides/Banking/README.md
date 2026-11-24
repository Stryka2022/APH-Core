# APH Havoc Banking

An in-game bank for DayZ 1.30:

- ATMs that staff place anywhere
- deposits and withdrawals of your server's money items
- player-to-player transfers
- a transaction history
- an optional paycheck
- a full admin page for accounts, currency and settings

Other APH mods take their fees from the bank: Plot Pole upkeep, Spawns costs and Virtual Garage fees.

---

## Requirements

| Mod | Needed |
|---|---|
| APH Core + Community Framework (CF) | Required, on the server and every client |
| APH Havoc Advanced Clans | Optional. Shows ATMs on the map with a **$** marker. |

---

## Installation

1. Put `APH_Havoc_Banking.pbo` and its `.bisign` with your APH mods, and its key in the server's `keys` folder.
2. Load it on the server and every client, after APH Core.
3. Start the server once. It creates `Config\Banking\Settings.json` and `Data\Banking\` under `<profiles>\aph_mods\APH_System_Manager\`.
4. Give staff the permissions below in APH Core's `Config\Permissions.json`. Owner has them all.
5. In game, open the **Banking** tile:
   - On **Currency**, pick your money items.
   - On **ATMs**, place your ATMs.

---

## Players

Walk up to an ATM and choose **Use ATM**.

- **Balance:** deposit money items (an amount, or all your cash) and withdraw.
- **Transfer:** send money to an online player. The fee shows before you send.
- **History:** your last 30 transactions.

---

## Admins (open the APH shell, then the **Banking** tile)

| Page | What it does | Permission |
|---|---|---|
| **ATMs** | Name an ATM, pick its model, then **Place in world** with the APH placer (live preview; Left/Right rotate, Up/Down height, Enter places). Placed ATMs are saved, come back after every restart, and get a map marker. You can also teleport to, rename or delete an ATM. | `banking.admin` |
| **Accounts** | Search online or offline players, then set, add or remove money, or set an extra limit. | `banking.admin` |
| **Currency** | Which items count as money, and what one of each is worth. | `banking.currency` |
| **Settings** | Start money, max balance, fees, paycheck, ATM range, and the map marker's name, icon and colour. | `banking.admin` |

Changes apply live. **System > Reload configs > Banking** re-reads the files.

---

## Settings (`Config\Banking\Settings.json`)

| Setting | What it does |
|---|---|
| `BankName`, `CurrencySymbol` | Names shown in the ATM menu |
| `Currencies` | Money items and their value (edit on the Currency page) |
| `StartMoney`, `MaxBalance` | New account balance, and the most an account can hold |
| `TransfersEnabled`, `TransferFeeBase`, `TransferFeePercent` | Transfers and their fee |
| `PaycheckAmount`, `PaycheckMinutes` | Money paid to online players every N minutes (`0` = off) |
| `ATMClassNames`, `UseDistance` | Which objects work as ATMs, and how close you must be |
| `ShowATMsOnMap`, `MarkerName`, `MarkerIcon`, `MarkerColor` | The ATM map marker |

The `Help` list in the file explains every field.

| File | What it holds |
|---|---|
| `Data\Banking\Accounts\<steam64>.json` | One account per player |
| `Data\Banking\ATMs.json` | Placed ATMs |

---

## Discord

Banking posts through APH Core's shared webhooks (**System > Discord webhooks**):

- `Bank_TRANSFER`: a player sent money to another player.
- `Bank_ADMIN`: staff changed an account, an ATM or the bank settings.

Add the events to a webhook's `Events` list, or use `*`.

---

## Classnames

| Class | What it is |
|---|---|
| `ATM_1` | The ATM, placed by staff from the Banking tile |
| `ATM_Card_1` | An ATM card. It's an item for traders or loot; you don't need it to use an ATM. |

The ATM model is used under CC-BY-4.0; see `ATMS/ATM_1/README.md` for the attribution.