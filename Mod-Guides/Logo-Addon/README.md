# APH Havoc Logo Addon

Your server's own logo, title and link buttons (Discord, website, store ...) in the APH Clans map menu, plus an optional logo that stays on screen while players play. Staff edit it all in game.

---

## Requirements

| Mod | Needed |
|---|---|
| APH Core + Community Framework (CF) | Required, on the server and every client |
| APH Havoc Advanced Clans | Needed for the map menu logo and buttons. The on-screen logo works without it. |

---

## Installation

1. Put `APH_Havoc_Logo_Addon.pbo` and its `.bisign` with your APH mods, and its key in the server's `keys` folder.
2. Load it on the server and every client, after APH Core.
3. Start the server once. It creates `<profiles>\aph_mods\APH_System_Manager\Config\Logo\Logo.json`.
4. Give staff `logo.edit` in APH Core's `Config\Permissions.json`. Owner has it.
5. In game, open the **Logo** tile in the APH shell, put in your real links, and **Save for everyone**. The defaults are placeholders.

---

## Your own logo

- **Replace the image:** swap `APH_Havoc_Logo_Addon\GUI\logo_ca.paa` for your logo and repack the PBO. Convert it with ImageToPAA; the file name must end in `_ca` so the alpha is kept.
- **Or point at another texture:** set **Logo path** to any `.paa` in a mod your players load.
- **Any shape works:** the map menu fits the logo into a box up to 330 × 230 and keeps its own proportions. A square badge shows as a square, and a wide banner fills the width.

---

## In game (APH shell > **Logo**, permission `logo.edit`)

- **Logo path**, **Title** and **Subtitle**. **Preview** shows the logo before you save.
- **Show logo and buttons in the map menu:** the logo replaces the map's banner, and your buttons replace its links.
- **Keep the logo on screen:**
  - Pick the corner, the width (60-600, at 1080p) and the opacity.
  - It scales with the player's screen.
  - It hides while a menu is open.
- **Up to 8 buttons:** each has a Name, Subtitle, URL and Icon. The icons are `link`, `discord`, `home`, `store`, `info`, `map`, `shield`, `radio` and `leaderboard`. A button with no name is removed.

**Save for everyone** applies to every player at once. Editing `Logo.json` by hand works too; reload it with **System > Reload configs > Logo**.