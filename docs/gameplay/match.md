# How a match works

A match goes through these states:

```text
WAITING → COUNTDOWN → STARTING → ACTIVE ⇄ MEETING → VOTING → ENDING → reset
```

Every match is played on a **temporary copy** of the arena's map world: when it ends, the copy is deleted and a fresh one is used next time.

## 1. Waiting lobby

Players join with `/iu join`, the arena menu (`/iu menu` or the compass in the hub), an `[IU]` sign or a Citizens NPC. In the waiting lobby they hold:

| Item | Default material | What it does |
|---|---|---|
| Colour | `WHITE_WOOL` | Pick your suit colour (12 colours: red, blue, green, pink, orange, yellow, black, white, purple, brown, cyan, lime). |
| Stats | `BOOK` | Your statistics (needs `iu.stats`). |
| Leave | `RED_BED` | Leave the match. |

In the waiting lobby everyone keeps their own skin: the coloured suit is put on only when the round starts.

Once `min-players` is reached the countdown starts (`auto-start`, `countdown-seconds` in `game.yml`). Staff can start at once with `/iu start`.

## 2. Role reveal

Roles are assigned in secret and shown on screen; players are frozen for `reveal-seconds`.

- The number of impostors comes from the `game.impostors` table (by number of players), or from the arena's fixed `players.impostors`.
- Admins can pick impostors beforehand with `/iu impostor <player>` or pin any role with `/iu admin role` (see [Admin tools](../reference/admin-tools.md)).
- Players with [streamer mode](streamer-mode.md) on are never drawn as impostor.

### Roles

| Role | Faction | What it does |
|---|---|---|
| **Crewmate** | Crew | Does tasks, reports bodies, votes. |
| **Impostor** | Impostors | Kills, sabotages, uses vents, gets a list of fake tasks. |
| **Sheriff** (optional) | Crew | One shot: hitting an impostor kills them; shooting an innocent kills the sheriff instead. The shot is not counted as a kill. |
| **Engineer** (optional) | Crew | Can use vents like the impostors (`roles.engineer.can-vent`). |

The optional roles are off by default (`roles.yml`). Medic and scientist exist in the files but their abilities are **not implemented yet**: leave them off. Win conditions always depend on the faction, never on the role.

## 3. Free play

- Everyone spawns holding the **task list**. Impostors receive the kill item `kill-item-delay-seconds` (default 2) after the spawn.
- **Crewmates** do their [tasks](tasks.md), each a minigame with its own interface.
- **Impostors** kill (after the kill cooldown, within `kill-distance`, with a left or right click), [sabotage](sabotages.md) and use [vents](vents.md). The first kill cooldown is `first-kill-cooldown-seconds`.
- Name tags and skins are hidden: you recognise people by their suit colour. See [Anonymity](anonymity.md).
- Everyone carries a [ship map](ship-map.md) in the off hand (once an admin has rendered it).
- A killed player leaves a **body**; its name is not shown above it (`show-corpse-names: false`), so you recognise it by the colour.
- Impostors recognise each other with a crown of red particles (`impostor-markers`) and, with ProtocolLib, an optional glow through walls (`impostor-glow`).

By default the living can't chat during free play: see [Chat](../features/chat.md).

## 4. Meetings

Reporting a body or pressing the emergency button seats everyone at the meeting table: discussion, then voting. The most voted player is ejected; a tie or a skip ejects nobody. See [Meetings & voting](meetings.md).

Killed and ejected players become [ghosts](ghosts.md).

## 5. End of the match

| Winner | How |
|---|---|
| **Crew** | Every task is finished (ghosts' tasks count too), or every impostor is ejected. |
| **Impostors** | They match the crew in number, enough innocents are ejected to leave the impostors in the majority, or a critical sabotage (reactor, oxygen) is not repaired in time. |
| **Nobody** | Not enough players left, or an admin stops the match / ends it as a `draw`. |

The end screen lasts `end-delay-seconds`. Rewards (XP, coins, reward commands) are handed out, then the arena copy is deleted. See [Stats, levels & rewards](../features/progression.md).

## Disconnects and rejoin

A disconnected player can rejoin within `rejoin-grace-seconds` (default 45) without being eliminated; `0` eliminates them at once. A player rejoining during a meeting lands on their own seat. Players rejoining a running match are not pulled to the hub by `spawn-on-join`.

## During a match

- Other commands are blocked (`commands.block-in-game` in `config.yml`), so nobody can teleport away or pass on information. `iu.admin.bypass` is exempt.
- Ender pearls and chorus fruit are blocked (`block-teleport-items`).
- The match world has a fixed time (`world-time`).
- Pressure plates, tripwires and farmland don't react to players in a match.
