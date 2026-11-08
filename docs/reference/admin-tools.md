# Admin tools

Staff tools to watch, steer and debug matches. Every `/iu admin <tool>` needs **`iu.admin.match`** and works **from the console too** (except `menu`, `tp`, `alerts` and `spectate`), so tournaments can be scripted.

Commands with an optional `[arena]` use, when it's left out, the match you are in or watching, otherwise the only arena.

## Watching a match

| Command | What it does |
|---|---|
| `/iu admin menu` | Admin panel GUI: arenas → match (players, start/stop, meeting, vote, sabotages, pause, end) → player (tp, eliminate, revive, tasks, mute, kick). Alias: `/iu admin gui`. |
| `/iu admin info [arena]` | Full state of a match: time, alive players, tasks, sabotage, meeting timer, every player with colour, role, status, tasks and kills (click a name for their page). |
| `/iu admin player <player>` | One player's page: arena, role, status, every task, vote, cooldowns, with clickable action buttons. |
| `/iu admin arenas` | Every arena with state, players, match world and whether it is ready. |
| `/iu admin alerts` | Live staff alerts in chat (kills, reports, meetings, ejections, sabotages, admin actions) from every match you are not playing in. On/off. |
| `/iu admin spectate [arena]` | Watch a running match invisibly without playing. Run it again, or `/iu leave`, to stop. You get no plugin sidebar while watching. |
| `/iu admin tp <player>` | Jump to a match player, watching invisibly. |

## Steering a match

| Command | What it does |
|---|---|
| `/iu start [arena]` / `/iu stop [arena]` | Force the start / stop a match (`iu.admin.game`). |
| `/iu admin meeting [arena]` | Open an emergency meeting (free play only). |
| `/iu admin endvote [arena]` | During the discussion: open the vote now. During the vote: count the votes now. |
| `/iu admin sabotage <lights\|communications\|reactor\|oxygen\|doors\|fix> [arena]` | Start a sabotage (ignores the cooldown, needs the arena's panels) or repair the running one. |
| `/iu admin kill <player>` | Eliminate a player without leaving a body. |
| `/iu admin revive <player>` | Bring a player back to life where they stand. |
| `/iu admin end <crew\|impostors\|draw> [arena]` | End the match with the winner you choose (`draw` = no winner). Stats and rewards are recorded for crew/impostors. |
| `/iu admin pause [arena]` | Freeze the match: timers, cooldowns, sabotages, rejoin windows; players can't move or act. Again to resume. |
| `/iu admin settime <seconds> [arena]` | Change the running timer: lobby countdown, discussion, vote, or critical sabotage. |
| `/iu admin task <player> <complete\|reset>` | Complete or reset a player's tasks (e.g. a stuck task). |
| `/iu admin broadcast <arena> <message>` | Staff message to everyone in one match. |
| `/iu admin mute <player>` | Mute / unmute a player in the match chat (this match only). |

## Picking roles

| Command | What it does |
|---|---|
| `/iu impostor <player> [arena]` | Toggle a player of a **waiting lobby** as a forced impostor (at most as many as the impostors planned for that lobby size). |
| `/iu impostor list\|clear [arena]` | Show / clear the forced impostors. |
| `/iu admin role <player> <role\|clear>` | Pin a role for the next round (lobby only): `crewmate`, `impostor`, `sheriff`, `engineer`, `medic`, `scientist`. |

An admin's `/iu impostor` pick wins over [streamer mode](../gameplay/streamer-mode.md).

## Match log

With `admin.match-log: true` (`config.yml`, default on) every match is written when it ends to:

```text
plugins/MRTImposterUs/logs/<arena>-<date>.txt
```

It contains players and roles, kills, reports, meetings, every vote, ejections, sabotages and admin actions, with **real names** and times. Useful to check a player's report after the fact.

## Debug

`/iu debug` (`iu.admin.debug`) turns detailed console logging on or off **until restart** (the permanent switch is `debug` in `config.yml`). When turned on it prints the current state of every arena straight away. It also logs vents, tab list, streamer immunity, the Players item, the kill move and the resource pack.

### Debug report

```text
/iu debug report
```

Writes `plugins/MRTImposterUs/reports/report-<date>.txt` with the server, plugins, arenas and configuration. Database credentials are replaced with `<hidden>`. **Attach this file when asking for help.**

## Update checker

`update-checker` in `config.yml`:

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `true` | Checks Modrinth at startup and every 6 hours. Nothing is downloaded. |
| `notify-admins` | `true` | Admins with `iu.admin.notify` are told about new versions when they join. |
| `include-beta` | `false` | Also report beta versions. |

## bStats

`metrics: true` sends anonymous usage statistics to [bStats](https://bstats.org): server count, versions and a few settings, never names or IPs. Turn it off here, or for every plugin in `plugins/bStats/config.yml`.
