# Stats, levels & rewards

Every player has statistics, a level, XP and coins. They are stored in a database (SQLite by default, or MySQL) and are always **global**, never per arena.

## Storage

Configured in the `stats` section of `config.yml`: see [Storage: SQLite or MySQL](../getting-started/installation.md#storage-sqlite-or-mysql).

- Every database read and write goes through **one database thread, in order**, and pending writes are flushed on shutdown.
- With MySQL, idle connections are re-opened automatically. The MySQL driver must be provided by the server.
- The `stats` section is read at startup only: restart after changing it.

## Statistics

| Statistic | Leaderboard category |
|---|---|
| Wins (crew + impostor) | `wins` |
| Wins as crewmate | `wins_crew` |
| Wins as impostor | `wins_impostor` |
| Kills | `kills` |
| Deaths | `deaths` |
| Games played | `games` |
| Games as impostor | `games_impostor` |
| Tasks completed | `tasks` |
| Bodies reported | `reports` |
| Sabotages fixed | `sabotages_fixed` |
| Correct votes | `correct_votes` |
| Times ejected | `times_ejected` |
| XP | `xp` |
| Coins | `coins` |

| Command | What it does | Permission |
|---|---|---|
| `/iu stats` | Your statistics (also the stats item in the hub and waiting lobby). | `iu.stats` |
| `/iu stats <player>` | Another player's statistics (online, or offline if known to the server). | `iu.stats.others` |
| `/iu top [category]` | Top 10 of a category (default `wins`). | `iu.top` |

The `%imposterus_top_<category>_<n>_name%` / `_value%` [placeholders](../reference/placeholders.md#leaderboards) show the leaderboards in holograms or the tab list. They are read from a cache refreshed every `stats.leaderboard-refresh-seconds` (default 60, minimum 5).

## XP, levels and coins (`economy.yml`)

```yaml
progression:
  # XP needed to level up.
  xp-per-level: 500
  xp:
    play: 20              # just for playing the match
    win: 100              # victory of your own faction
    per-kill: 15          # each kill (impostors)
    per-task: 10          # each completed task (crewmates)
    per-correct-vote: 20  # each correct vote against an impostor
  coins:
    play: 10
    win: 50
    per-kill: 8
    per-task: 5
```

XP and coins are awarded at the end of the match; XP determines the level. `progression` is always global (it can't be changed per arena).

!!! note "No shop"
    Coins are tracked per player (`/iu coins`, `%imposterus_coins%`) for **other plugins** to spend: MRTImposterUs itself has no shop.

Staff can change XP and coins with `/iu xp <give|take|set> <player> <amount>` and `/iu coins <give|take|set> <player> <amount>` (`iu.admin.economy`). Negative amounts are refused.

## Reward commands

Console commands run at the end of the match. Use `%player%` (or `<player>`) for the name. They are the way to hook up any economy, rank or kit plugin.

```yaml
rewards:
  # At the end of the match, one for each winner.
  winner-commands: []
  # For each kill (%player% = the impostor who killed).
  kill-commands: []
```

Examples:

```yaml
rewards:
  winner-commands:
  - 'eco give %player% 100'
  - 'lp user %player% permission settemp rank.vip true 1d'
```

`rewards` can be changed [per arena](../getting-started/arena-settings.md).

## Resetting the statistics

```text
/iu resetstats <stats|all>
```

| Mode | What is deleted |
|---|---|
| `stats` | Zeroes the match statistics of every player. Level, XP and coins are kept. |
| `all` | Deletes every row. |

1. The first command shows what will be deleted and a **confirmation code**.
2. Confirm within **60 seconds** with `/iu resetstats <mode> confirm <code>` (click the line in chat to put the command in the chat box).

- Refused while a match is running.
- A **backup** is always taken first: `backups/stats-<date>.db` for SQLite, an `iu_stats_backup_<date>` table for MySQL.
- The streamer immunities (`iu_streamers`) are not touched.
- Needs **`iu.admin.resetstats`**, which is **not** part of `iu.admin`.
