# Scoreboard

MRTImposterUs draws its own sidebar scoreboard. You don't need TAB or PlaceholderAPI for it. The board changes with the situation:

| Board | Shown when |
|---|---|
| `hub` | in the hub / lobby world, not in a match |
| `waiting` | in an arena's waiting lobby |
| `countdown` | the match is about to start |
| `match` | playing (role reveal, free play) |
| `meeting` | a meeting is running (discussion or voting) |
| `ghost` | dead or ejected, finishing the tasks as a ghost |
| `ending` | end-of-match screen |

Admins watching a match with `/iu admin spectate` get no plugin sidebar. Outside a match the sidebar doesn't replace another plugin's scoreboard.

Every board is **personal**: what one player sees is never sent to anyone else. That is why the board can show your own role (`<role>`) and your kill cooldown without spoiling the game.

## Settings: `config.yml`

```yaml
scoreboard:
  enabled: true          # false = no plugin scoreboard (e.g. you keep TAB's)
  update-ticks: 10       # refresh rate (20 = 1 second), minimum 5
  hub-worlds: []         # worlds with the "hub" board; empty = the lobby world
  date-format: 'dd/MM/yyyy'
  server-address: 'play.yourserver.com'   # the <server> placeholder
  boards:                # turn single boards on/off
    hub: true
    waiting: true
    countdown: true
    match: true
    meeting: true
    ghost: true
    ending: true
```

!!! warning "Only one sidebar at a time"
    If you use the **TAB** plugin, turn off its scoreboard feature or set `scoreboard.enabled: false` here (and build your board in TAB with the `%imposterus_*%` [placeholders](../reference/placeholders.md) and `%imposterus_board%`). TAB's tab list, name tags and other features keep working.

Your personal board keeps the name colours, the hidden name tags and the glow colours: the teams of the main scoreboard (this plugin's and other plugins') are copied into it whenever they change.

## Texts: `languages/messages_<lang>.yml`

Titles and lines live in the `scoreboard.boards` section of the messages file, so they follow the plugin language:

```yaml
scoreboard:
  boards:
    match:
      title: '<gradient:#ff4d4d:#c51111><bold>ImposterUs</bold></gradient>'
      lines:
        - '<gray>Map <dark_gray>» <white><arena>'
        - '<gray>Role <dark_gray>» <role>'
        - ''
        - '<gray>Tasks <dark_gray>» <green><tasks_percent>%'
        - '<kill_status>'
        - '<sabotage_status>'
```

- Formatting is [MiniMessage](https://docs.advntr.dev/minimessage/format.html), like the rest of the plugin.
- At most **15 lines**.
- `''` is an empty spacer line.
- A line made **only of placeholders** that ends up empty is hidden. Lines with any literal text stay. For example, `<kill_status>` disappears for crewmates, and `<sabotage_status>` disappears when no sabotage is running.
- With `messages.smallcaps: true` the literal text becomes small caps (placeholder values never do).
- With PlaceholderAPI installed, any `%placeholder%` works too (its value is inserted as plain text).
- Apply changes with `/iu reload`.

The texts of a few placeholders are in the same section: `none`, `role-hidden`, `comms-hidden`, `kill-ready`, `kill-cooldown`, `sabotage-active`, `sabotage-timer`, `meeting-discussion`, `meeting-voting`, `winner-crew`, `winner-impostor`, `winner-none`, `result-win`, `result-lose`.

## Placeholders

Always available:

| Placeholder | Value |
|---|---|
| `<player>` | player name |
| `<online>` | online players |
| `<date>` | today's date (`date-format`) |
| `<server>` | your server address (`scoreboard.server-address`) |
| `<level>` `<xp>` `<coins>` | progression |
| `<xp_progress>` `<xp_target>` `<xp_percent>` | XP in the current level, XP needed, percent |
| `<wins>` `<wins_crew>` `<wins_impostor>` | lifetime wins (total, as crewmate, as impostor) |
| `<kills_total>` `<deaths>` `<games>` | lifetime stats |
| `<kd>` `<winrate>` `<tasks_completed>` | lifetime stats |

In an arena or match (outside one they show `-` or `0`):

| Placeholder | Value |
|---|---|
| `<arena>` | arena name |
| `<state>` | match state (Waiting, Countdown, In progress, Meeting...) |
| `<players>` `<max_players>` `<min_players>` `<needed>` | queue |
| `<countdown>` | seconds to the start |
| `<impostors>` | impostors of the match (before the reveal: how many there will be) |
| `<alive>` `<ghosts>` | living players / dead players |
| `<color>` | your suit colour, in its colour |
| `<role>` | **your** role, translated; `?` (`role-hidden`) before the reveal |
| `<kills>` | your kills in this match |
| `<kill_status>` | impostors only: "Kill » ready" or "Kill » 12s"; empty for the crew |
| `<kill_cooldown>` | seconds to your next kill |
| `<emergencies>` | emergency meetings you have left |
| `<tasks_percent>` `<tasks_done>` `<tasks_total>` | crew task progress |
| `<tasks_bar>` | 10-segment task bar |
| `<my_tasks>` | your tasks, e.g. `3/6` |
| `<sabotage>` | running sabotage (name) or `-` |
| `<sabotage_timer>` | seconds left of a critical sabotage (0 without a countdown) |
| `<sabotage_status>` | "⚠ Reactor 25s" while a sabotage runs; empty otherwise |
| `<meeting_phase>` | Discussion / Voting |
| `<meeting_time>` | seconds left of the current meeting phase |
| `<voted>` `<voters>` | living players who already voted / living players |
| `<winner>` | end screen: Crewmates / Impostors / Nobody |
| `<result>` | end screen: Victory / Defeat for you |
| `<xp_earned>` `<coins_earned>` | end screen: the reward of this match |

While **communications are sabotaged** the task placeholders (`<tasks_percent>`, `<tasks_done>`, `<tasks_total>`, `<tasks_bar>`, `<my_tasks>`) show `?` (`comms-hidden`), exactly like the task bar.

!!! tip "Don't reveal kills"
    The default `match` board does not show `<alive>` or `<ghosts>`: knowing how many players are still alive would tell everyone a kill just happened. The `meeting` board shows them, since a meeting reveals the dead anyway.
