# Permissions

All permissions are declared in `plugin.yml`, so permission plugins such as LuckPerms show them with a description. They are organised in groups: a group grants everything below it.

```text
iu.*                         (op)        everything
├── iu.player                (everyone)  every player permission
│   ├── iu.play
│   ├── iu.stats
│   ├── iu.stats.others
│   └── iu.top
├── iu.admin                 (op)        every staff permission
│   ├── iu.admin.setup
│   ├── iu.admin.game
│   ├── iu.admin.match
│   ├── iu.admin.build
│   ├── iu.admin.economy
│   ├── iu.admin.npc
│   ├── iu.admin.signs
│   ├── iu.admin.reload
│   ├── iu.admin.debug
│   ├── iu.admin.bypass
│   ├── iu.admin.notify
│   └── iu.chat.bypass
├── iu.admin.resetstats      (op)        NOT part of iu.admin
└── iu.streamer              (nobody)    NOT part of iu.player
```

| Permission | Default | Grants |
|---|---|---|
| `iu.*` | op | Everything: `iu.player`, `iu.admin`, `iu.admin.resetstats` and `iu.streamer`. |
| `iu.player` | everyone | Every player permission. |
| `iu.admin` | op | Every staff permission, plus `iu.chat.bypass`. |

!!! warning "`iu.admin.resetstats` is not in `iu.admin`"
    `/iu resetstats` wipes the statistics of **every** player, so it is deliberately left out of `iu.admin`. Only `iu.*` grants it, or you give the node by hand.

## Players (everyone has them by default)

| Permission | What it allows |
|---|---|
| `iu.play` | Join matches: `/iu join`, `/iu menu`, `/iu list`, arena NPCs and `[IU]` signs. Leaving (`/iu leave`) is always allowed. |
| `iu.stats` | Your own statistics: `/iu stats` and the stats item. |
| `iu.stats.others` | Other players' statistics: `/iu stats <player>`. |
| `iu.top` | Leaderboards: `/iu top`. |

Private messages (`/iu msg`, `/iu r`) and `/iu help` need no permission; they follow the `chat.private-messages` settings of `config.yml`.

To take something away from players, negate it. For example, with LuckPerms `lp group default permission set iu.top false` hides the leaderboards.

## Streamers

| Permission | Default | What it allows |
|---|---|---|
| `iu.streamer` | nobody | `/iu streamer`: on/off immunity from being drawn as impostor. Give it to your streamers. See [Streamer mode](../gameplay/streamer-mode.md). |

## Staff

| Permission | What it allows |
|---|---|
| `iu.admin.setup` | Arena setup: `/iu setup ...`, the setup GUI, `/iu setlobby`. |
| `iu.admin.game` | `/iu start`, `/iu forcestart`, `/iu stop`, `/iu forcejoin`, `/iu kick`. |
| `iu.admin.match` | Match tools: `/iu impostor <player\|clear\|list>` and every `/iu admin <tool>` (menu, info, player, arenas, alerts, spectate, tp, meeting, endvote, sabotage, kill, revive, end, pause, settime, role, task, broadcast, mute). |
| `iu.admin.build` | Build mode in the hub (`/iu admin`) and building in the hub while it is on. |
| `iu.admin.economy` | `/iu coins` and `/iu xp` (give / take / set). |
| `iu.admin.npc` | Arena NPCs: `/iu npc`. |
| `iu.admin.signs` | Creating `[IU]` join signs. |
| `iu.admin.reload` | `/iu reload`. |
| `iu.admin.debug` | `/iu debug` and `/iu debug report`. |
| `iu.admin.bypass` | Not blocked inside matches: commands and teleports. Building always follows the map rule. |
| `iu.admin.notify` | Told about new plugin versions when joining. |
| `iu.admin.resetstats` | `/iu resetstats`: resets the statistics of every player. **Not** in `iu.admin`. |
| `iu.chat.bypass` | No chat anti-flood limits. |

## Examples (LuckPerms)

A moderator who runs matches and picks impostors for events, but cannot touch arenas or coins:

```text
lp group mod permission set iu.admin.match true
lp group mod permission set iu.admin.game true
```

A builder who only sets up arenas:

```text
lp group builder permission set iu.admin.setup true
```

A streamer:

```text
lp user <name> permission set iu.streamer true
```
