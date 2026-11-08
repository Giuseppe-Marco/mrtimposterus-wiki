# Placeholders

With **PlaceholderAPI** installed, MRTImposterUs registers the identifier **`imposterus`**: format `%imposterus_<key>%`. No expansion download is needed: the console shows `Hooked into PlaceholderAPI` at start.

- Match placeholders are **relative to the player** (the match they are in). When the player is not in a match they return `NONE` / `0`, never an empty string (which would break TAB's conditions and number formats).
- Translated texts (state, role, sabotage, Yes/No) follow the `language` of `config.yml`. Yes/No come from `placeholder.bool-true` / `placeholder.bool-false` in the messages file.
- Requests made off the main thread (TAB resolves asynchronously) are answered from a snapshot rebuilt on the main thread every 10 ticks, so they never touch the live match.
- The common keys (board, state, arena, players, level, xp, coins, wins, kills, games, in_match, name_color...) are the same as the MRTBattleRoyale expansion (`brbattle`): a TAB layout written for one plugin only needs the identifier swapped.

!!! tip "The sidebar doesn't need them"
    The plugin draws its **own sidebar** without PlaceholderAPI or TAB, with its own `<placeholders>`: see [Scoreboard](../features/scoreboard.md).

## Board (for TAB's display-condition)

| Placeholder | Description | Values |
|---|---|---|
| `%imposterus_board%` | Which screen to show | `HUB`, `LOBBY`, `MATCH`, `MEETING`, `GHOST` |

This gives **a different scoreboard for every phase** in TAB: hub, waiting, match, meeting and ghosts.

## Match (relative to the player)

| Placeholder | Description | Examples |
|---|---|---|
| `%imposterus_state%` | Internal state | `WAITING`, `COUNTDOWN`, `ACTIVE`, `MEETING`, `VOTING`, `NONE` |
| `%imposterus_state_name%` | Translated state | `In progress`, `Meeting` |
| `%imposterus_arena%` | Arena name | `Skeld`, `NONE` |
| `%imposterus_players%` | Players in the match | `8` |
| `%imposterus_player_count%` | Alias of `players` | `8` |
| `%imposterus_min%` | Minimum players | `4` |
| `%imposterus_max%` | Maximum players | `10` |
| `%imposterus_needed%` | How many more are needed to start | `2` |
| `%imposterus_alive%` | Living players | `6` |
| `%imposterus_countdown%` | Seconds to the start | `30` |
| `%imposterus_crew_alive%` | Living crewmates | `5` |
| `%imposterus_impostors_alive%` | Living impostors | `1` |
| `%imposterus_impostors_total%` | Impostors of the match | `2` |
| `%imposterus_ghosts%` | Ghosts (dead players) | `2` |
| `%imposterus_meeting_timer%` | Seconds left of discussion / voting | `45` |

!!! warning "Spoilers"
    `crew_alive`, `impostors_alive` and `ghosts` **reveal game information**: use them only in the `GHOST` board, never in `MATCH`.

## Tasks

| Placeholder | Description | Examples |
|---|---|---|
| `%imposterus_tasks_percent%` | Crew completion (%) | `62` |
| `%imposterus_tasks_done%` | Steps completed | `15` |
| `%imposterus_tasks_total%` | Total steps | `24` |
| `%imposterus_tasks_bar%` | 10-segment bar | `▮▮▮▮▮▮▯▯▯▯` |

## Sabotage

| Placeholder | Description | Examples |
|---|---|---|
| `%imposterus_sabotage%` | Running sabotage (translated name) | `Reactor`, `NONE` |
| `%imposterus_sabotage_timer%` | Seconds left (0 when it has no countdown) | `87` |
| `%imposterus_sabotage_active%` | Is a sabotage running? | `Yes`, `No` |
| `%imposterus_sabotage_critical%` | Is it a critical one (reactor, oxygen)? | `Yes`, `No` |

## Player (in a match)

Always about the player the placeholder is resolved for: nobody else's role is ever exposed.

| Placeholder | Description | Examples |
|---|---|---|
| `%imposterus_is_alive%` | Are you alive? | `Yes`, `No` |
| `%imposterus_match_kills%` | Kills in this match | `2` |
| `%imposterus_in_match%` | `1` from the role reveal to the end screen, else `0` (numeric, for TAB's disable-condition) | `1` |
| `%imposterus_name_hex%` | Suit colour as `#rrggbb` (empty without a colour) | `#c51111` |
| `%imposterus_name_color_hex%` | Suit colour as `&#rrggbb`, for TAB prefixes | `&#c51111` |
| `%imposterus_name_color%` | Nearest legacy code of the suit colour | `&c` |
| `%imposterus_role%` | Internal role (`NONE` before the reveal) | `IMPOSTOR`, `CREWMATE` |
| `%imposterus_role_name%` | Translated role | `Impostor`, `Crewmate` |
| `%imposterus_team%` | Faction | `CREW`, `IMPOSTOR`, `NONE` |
| `%imposterus_color%` | Chosen colour | `NONE` when none |
| `%imposterus_color_hex%` | Colour in hex | `#c51111` |
| `%imposterus_my_tasks%` | Your tasks | `3/6` |
| `%imposterus_my_tasks_done%` | Tasks completed | `3` |
| `%imposterus_my_tasks_total%` | Tasks assigned | `6` |
| `%imposterus_kill_cooldown%` | Seconds to the next kill | `18` |
| `%imposterus_emergencies_left%` | Emergency meetings left | `1` |
| `%imposterus_is_ghost%` | Are you a ghost? | `Yes`, `No` |
| `%imposterus_is_impostor%` | Are you an impostor? (only after the reveal) | `Yes`, `No` |
| `%imposterus_is_venting%` | Are you in a vent? | `Yes`, `No` |
| `%imposterus_match_tasks%` | Tasks done in this match | `4` |
| `%imposterus_match_reports%` | Bodies reported in this match | `1` |

!!! danger "Private information"
    Never put `role`, `team`, `is_impostor` or `kill_cooldown` in a text shown to **other** players (a broadcast, a hologram). TAB scoreboards are per player, so there they are fine. `%imposterus_role%` stays `NONE` until the roles are assigned, so the tab list gives nothing away in the waiting lobby.

## A named arena (selectors / holograms)

Format: `%imposterus_arena_<what>_<arena>%`, where `<what>` is one of `state`, `state_name`, `players`, `min`, `max`, `alive`, `needed`. The player doesn't need to be in a match.

| Placeholder | Description | Examples |
|---|---|---|
| `%imposterus_arena_state_skeld%` | State of arena `skeld` | `WAITING`, `NONE` |
| `%imposterus_arena_state_name_skeld%` | Translated state | `Waiting` |
| `%imposterus_arena_players_skeld%` | Players in the arena | `4` |
| `%imposterus_arena_min_skeld%` / `_max_skeld` | Player limits | `4` / `10` |
| `%imposterus_arena_alive_skeld%` | Living players | `6` |
| `%imposterus_arena_needed_skeld%` | How many more are needed to start | `2` |

## Global (always available, even without a player)

| Placeholder | Description | Examples |
|---|---|---|
| `%imposterus_total_players%` | Players in every match | `24` |
| `%imposterus_arenas%` | Configured arenas | `3` |
| `%imposterus_active_games%` | Matches in progress | `2` |
| `%imposterus_waiting_games%` | Matches waiting for players | `1` |
| `%imposterus_total_online%` | Players online on the server | `37` |

## Leaderboards

`%imposterus_top_<category>_<n>_name%` and `%imposterus_top_<category>_<n>_value%`, with `n` from 1 to 10. Always available, even without a player. Read from a cache refreshed every `stats.leaderboard-refresh-seconds` (default 60, minimum 5): never a query on the main thread. An empty row gives `---` / `0`.

| Placeholder | Description | Examples |
|---|---|---|
| `%imposterus_top_wins_1_name%` | First in the wins leaderboard | `Steve` |
| `%imposterus_top_wins_1_value%` | Their value | `42` |
| `%imposterus_top_kills_3_name%` | Third by kills | `Alex` |
| `%imposterus_top_tasks_10_value%` | Tenth by tasks completed | `120` |

Categories: `wins`, `kills`, `deaths`, `games`, `xp`, `coins`, `wins_crew`, `wins_impostor`, `games_impostor`, `tasks`, `reports`, `sabotages_fixed`, `correct_votes`, `times_ejected`. Aliases: `level` (= `xp`), `crew`, `impostor`, `votes`, `ejected`. In placeholders an unknown category is ranked by wins.

## Profile / statistics

| Placeholder | Description | Examples |
|---|---|---|
| `%imposterus_level%` | Level | `12` |
| `%imposterus_xp%` | Total XP | `5840` |
| `%imposterus_xp_progress%` | XP in the current level | `340` |
| `%imposterus_xp_target%` | XP per level | `500` |
| `%imposterus_xp_percent%` | Percent towards the next level | `68` |
| `%imposterus_coins%` | Coins | `1250` |
| `%imposterus_wins%` | Total wins (crew + impostor) | `19` |
| `%imposterus_games%` | Games played | `52` |
| `%imposterus_kills%` | Total kills | `23` |
| `%imposterus_deaths%` | Total deaths | `31` |
| `%imposterus_winrate%` | Win percentage | `36.5` |
| `%imposterus_kd%` | Kill/death ratio | `0.74` |
| `%imposterus_wins_crew%` | Wins as crewmate | `14` |
| `%imposterus_wins_impostor%` | Wins as impostor | `5` |
| `%imposterus_games_impostor%` | Games as impostor | `11` |
| `%imposterus_tasks%` | Tasks completed overall | `187` |
| `%imposterus_reports%` | Bodies reported | `14` |
| `%imposterus_sabotages_fixed%` | Sabotages fixed | `21` |
| `%imposterus_correct_votes%` | Correct votes | `9` |
| `%imposterus_times_ejected%` | Times ejected | `6` |

Profile placeholders read the profile loaded when the player joined; for a player not in the cache they return `0`.

## Names and colours with the TAB plugin

In a match the plugin colours every name with the player's suit colour and hides the name tags above the heads (`hide-nametags` in `game.yml`). It does this with scoreboard teams (named `iu...`). If TAB's **`scoreboard-teams`** is on, TAB manages the names itself and blocks the plugin (`plugins/TAB/anti-override.log` shows lines like *"Blocked attempt to add player ... into team iu..."*).

Tell TAB not to manage players **while they are in a match**: in `plugins/TAB/config.yml`, in the `scoreboard-teams` section, add the condition with `|` (= or):

```yaml
scoreboard-teams:
  enabled: true
  ...
  disable-condition: '%world%=disabledworld|%imposterus_in_match%=1'
```

Then `/tab reload`. Outside a match (hub, waiting lobby) TAB keeps managing names and sorting; in a match the plugin does.

The colour in the tab list comes from the team by itself when TAB's `tablist-name-formatting` is off. If you use it, add the colour to the list prefix:

```yaml
# plugins/TAB/groups.yml
_DEFAULT_:
  tabprefix: "%imposterus_name_color_hex%"
```

!!! tip "Building the sidebar in TAB"
    If you prefer TAB's scoreboard, set `scoreboard.enabled: false` in `config.yml` and use `%imposterus_board%` in TAB's `display-condition` to show one board per phase. The plugin's source ships a ready-made example, `docs/TAB-scoreboard-example.yml`.
