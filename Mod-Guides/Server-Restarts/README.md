# APH Havoc Server Restarts

Scheduled server restarts for DayZ 1.30 on APH Core:

- a restart schedule: fixed times of day, every few hours, or hours after the server started
- a countdown clock at the top of the screen (turns yellow, then red and blinking near the end)
- warnings at 30, 15, 10, 5, 2 and 1 minutes and at zero, each with its own spoken sound and an in-game notification
- a "server online" sound and message for players who join just after a restart
- Discord messages through a webhook: server online, every warning, and admin restarts, delays and skips
- APH Havoc Advanced Clans support: warnings also appear in APH Chat
- only staff can join in the last two minutes, then everyone is kicked and the server closes
- a **Restarts** page in the System Manager: restart in X minutes, delay, skip, back to schedule, test, and every setting

---

## Requirements

| Mod | Needed |
|---|---|
| APH Core + Community Framework (CF) | Required, on the server and every client |
| APH Havoc Advanced Clans | Optional. Warnings also go to APH Chat. |

---

## Installation

1. Put `APH_Havoc_Server_Restarts.pbo` and its `.bisign` with your APH mods, and its key in the server's `keys` folder.
2. Load it on the server and every client, after APH Core.
3. Start the server once. It creates `Config\ServerRestarts\ServerRestarts.json` under `<profiles>\aph_mods\APH_System_Manager\`.
4. Give staff the `restarts.manage` permission in APH Core's `Config\Permissions.json` (Owner has every permission).
5. With **ShutdownAtZero** on (the default), the mod closes the server at zero: your host or restart script must start it again.

---

## In game

**System Manager > Restarts > Restart schedule** (staff with `restarts.manage`):

| Button | What it does |
|---|---|
| **Restart in** | Restart in the number of minutes typed in the box. Every player is told. |
| **Delay by** | Push the next restart back by that many minutes. |
| **Skip next restart** | Go to the scheduled restart after the next one. |
| **Back to schedule** | Undo Restart in, Delay and Skip. |
| **Test** | Plays a warning sound and notification for you, a line in APH Chat, and a Discord message. |
| **Save for everyone** | Saves the settings below for the whole server. |

---

## ServerRestarts.json

| Setting | Default | What it does |
|---|---|---|
| `Enabled` | true | Restarts on or off. |
| `ScheduleMode` | `clock` | `clock` = times of day, `uptime` = hours after the server started. |
| `RestartIntervalHours` | 4 | Every this many hours (from 00:00 in clock mode). |
| `RestartOffsetMinutes` | 0 | Shifts the clock schedule: 30 gives 00:30, 04:30 ... |
| `RestartTimes` | [] | Fixed times, e.g. `["00:00", "06:00", "12:00", "18:00"]`. Used instead of the interval. |
| `UseUTC` | false | Times in UTC instead of the server PC's time. |
| `ShutdownAtZero` | true | Kick everyone and close the server at zero. |
| `LockBeforeSeconds` | 120 | Only staff can join in the last this many seconds. |
| `ShowClock`, `ClockAlwaysVisible` | true, true | The countdown at the top of the screen; off = only from yellow. |
| `ClockYellowSeconds`, `ClockRedSeconds` | 600, 120 | When the clock turns yellow, then red and blinking. |
| `PostToClanChat` | true | Warnings in APH Chat (APH Havoc Advanced Clans). |
| `DiscordWebhookUrl` | "" | Your Discord webhook. Empty = no Discord messages. |
| `DiscordUsername` | APH Havoc Restarts | The name the messages are posted under. |
| `DiscordServerOnline`, `DiscordWarnings` | true, true | Which messages go to Discord. |
| `ServerOnlineMinutes` | 15 | Players who join in the first minutes after a restart hear the online sound. |
| `ServerOnlineMessage`, `ServerOnlineSoundSet` | | The welcome message and sound. |
| `Warnings` | 7 entries | `Seconds` left, `Message` (`{time}` = the time left), `SoundSet`, `Discord`, `ToChat`. |

### Sounds

`APH_SR_30Min_SoundSet`, `APH_SR_15Min_SoundSet`, `APH_SR_10Min_SoundSet`, `APH_SR_5Min_SoundSet`,
`APH_SR_2Min_SoundSet`, `APH_SR_1Min_SoundSet`, `APH_SR_Now_SoundSet`, `APH_SR_Generic_SoundSet`,
`APH_SR_Online_SoundSet`. Any sound set from vanilla or another loaded mod works too.

### Live reload

System Manager > Reload > **Server Restarts** reloads `ServerRestarts.json` without a restart.