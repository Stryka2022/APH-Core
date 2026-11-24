# APH Havoc Killfeed

A full killfeed for DayZ 1.30:

- a sliding side feed with 3D weapon previews, hit zones, distances and streak badges
- a big centre notification for your own kills
- streak milestones
- a personal **look** for every player
- Discord kill logs

Everything is worked out on the server from vanilla game events.

---

## Requirements

- **APH Core** and **Community Framework (CF)**, on the server and every client. No additional event or loot frameworks are required.

---

## Installation

1. Put `APH_Havoc_Killfeed.pbo` and its `.bisign` with your APH mods, and its key in the server's `keys` folder.
2. Load it on the server and every client, after APH Core.
3. Start the server once. It creates `Config\Killfeed\Settings.json` and `Data\Killfeed\` under `<profiles>\aph_mods\APH_System_Manager\`.
4. Give staff `killfeed.edit` in APH Core's `Config\Permissions.json` to change settings in game. Owner has it.

---

## What players see

- **Side feed:** each kill slides in with:
  - the killer and victim, each with a player figure
  - the weapon in 3D
  - where the killing hit landed (HEAD, TORSO, ARMS, LEGS; **HEADSHOT** in gold)
  - the distance
  - a streak badge
  Deaths without a killer show their cause instead (BLED OUT, FELL ...).
- **Kill notification:** for your own kills, a big label (HEADSHOT, KILL, ROADKILL ...) with the weapon, victim and distance. Kills close together stack up to four.
- **Streak milestones:** ON FIRE (3), RAMPAGE (5), UNSTOPPABLE (8) and LEGENDARY (12) get their own band.
- **My look (F7):** each player picks how they appear when they get a kill:
  - a name colour (Spectrum, Inferno, Toxic, Neon, Gold Rush, Blood Moon, Aurora ...)
  - a slot style and a name motion
  - a player figure
  The same menu turns the side feed and kill notification on or off for that player only.
- **Moving it:** **Ctrl+H** (APH Core's HUD editor) moves the feed and the notification.

---

## How deaths are counted

| Kind | How |
|---|---|
| PvP, Headshot | Last firearm hit within 10 s. A head hit makes it a headshot. |
| Melee, Fists | Last close-combat hit |
| Explosion | Credited to whoever threw or placed it |
| Roadkill | Credited to the driver |
| Heli | Hit by a helicopter, or died in one without being shot |
| Infected, Wolf, Bear, Animal | Killed by them |
| Hunt | A player killed an animal |
| Fall, Suicide (with proof only), Bleeding, Hunger, Thirst, Sickness, Cold | From the player's state when nobody hit them |

- **Streak storage:** streaks are saved in `Data\Killfeed\Streaks.json`, so a crash doesn't lose them.
- **Streaks end:** on death, after `StreakIdleMinutes` (15) without a kill, or at a clean restart.

---

## Settings (`Config\Killfeed\Settings.json`)

Every field is explained in the file's `Help` list. The main ones:

- side feed and centre notification on/off
- `MaxSimultaneous` (1-8) and `LineSeconds`
- `MaxViewDistance` (`0` = everyone; the killer and victim always see their own kill)
- which kills show: AI, natural deaths, suicides, infected, animals
- `AnonymousNames` and `AITag`
- `PlayerLooks` and `PlayerFigures`, to turn player looks or figures off for everyone
- colours, feed side and position, milestones, and each death type's label, icon, colour and visibility

**In game:** in the APH shell, open **Killfeed > Killfeed settings** (`killfeed.edit`).

- **Save for everyone** applies to every player at once.
- **Show a test kill** shows a sample to you only.
- **Live reload:** **System > Reload configs > Killfeed**.

---

## Discord

Kills post through APH Core's shared webhooks (**System > Discord webhooks**):

- **Kill events:** `Kill_PVP`, `Kill_HEADSHOT`, `Kill_MELEE`, `Kill_EXPLOSION`, `Kill_ROADKILL`, `Kill_HELI`, `Kill_AI`, `Kill_SUICIDE`, `Kill_FALL`, `Kill_NATURAL`, `Kill_INFECTED`, `Kill_ANIMAL`
- **Streaks:** `Streak_MILESTONE`

Discord always gets the real names and Steam profile links, even when the in-game feed is anonymous. Point PvP events at a public kill channel and the rest at a staff log channel, for example.