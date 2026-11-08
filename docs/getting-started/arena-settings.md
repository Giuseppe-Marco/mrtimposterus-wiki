# Per-arena settings

Any arena can change the global rules **for itself only**. Add a `settings:` section to `arenas/<Name>.yml`, using the same paths as the global files:

```yaml
settings:
  game:
    kill-cooldown-seconds: 25
  tasks:
    long: 1
    short: 4
  meeting:
    discussion-seconds: 45
  rewards:
    winner-commands:
    - 'eco give %player% 500'
```

Then run `/iu reload`.

## What can be overridden

Everything a match reads from these sections:

| Section | Global file |
|---|---|
| `game` | `game.yml` |
| `tasks` | `tasks.yml` |
| `sabotage` | `sabotage.yml` |
| `meeting` | `meeting.yml` |
| `roles` | `roles.yml` |
| `sounds` | `game.yml` |
| `rewards` | `economy.yml` |
| `commands` | `config.yml` |

Keys you leave out keep the global value.

!!! note "Always global"
    `progression` (XP and coins) is always global, and so are the statistics.

!!! tip "Player limits"
    The `players.min` / `players.max` at the top of the arena file (set with `/iu setup min` / `max`) take precedence over `settings.game.min-players` / `max-players`. `players.impostors` set to a fixed value (with `/iu setup impostors <n>`) replaces the `game.impostors` table; `0` in the example file means the table is used.

## Example

The commented example `arenas/Skeld.yml.example` ends with a full `settings:` section showing every overridable key of `game`, `tasks`, `meeting`, `sabotage` and `roles`. For instance, a shorter rejoin window and a later first sabotage:

```yaml
settings:
  game:
    rejoin-grace-seconds: 10
  sabotage:
    initial-cooldown-seconds: 20
```

!!! warning "Common tasks"
    `tasks.common` should not be higher than the number of `COMMON` tasks the arena really has: otherwise the plugin warns at every match that the list will be shorter.
