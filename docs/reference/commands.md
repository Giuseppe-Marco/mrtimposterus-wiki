# Commands

Every command of the plugin is a sub-command of **`/iu`** (aliases **`/imposterus`** and **`/imposter`**). `/iu help` lists the commands; staff also get the admin part of the list.

!!! note "No top-level shortcuts"
    The plugin registers **only** `/iu` and its aliases. There is no `/join`, `/leave`, `/msg`, `/spawn`... of its own, so it never takes those commands away from the other plugins of your server.

!!! tip "Tab completion follows permissions"
    Tab completion only suggests the commands (and arguments) the sender is allowed to use. `/iu leave`, `/iu msg`, `/iu r` and `/iu help` never need a permission. See [Permissions](permissions.md).

**Notation:** `<required>`, `[optional]`, `a|b` = one of the values.

## Player commands

| Command | Description | Permission |
|---|---|---|
| `/iu help` | Shows the command list. | none |
| `/iu join` | With a single arena, joins it; otherwise joins the fullest available match (`quick-join` in `game.yml`) or opens the arena menu. | `iu.play` |
| `/iu join <arena>` | Joins the named arena. | `iu.play` |
| `/iu leave` | Leaves the match or the queue. Also stops `/iu admin spectate`. | none |
| `/iu menu` | Opens the arena selector. | `iu.play` |
| `/iu list` | Lists the arenas with state and players. | `iu.play` |
| `/iu stats` | Your own statistics. | `iu.stats` |
| `/iu stats <player>` | Another player's statistics (online, or offline if known to the server). | `iu.stats.others` |
| `/iu top [category]` | Top 10 leaderboard. Categories: `wins` (default), `kills`, `deaths`, `games`, `xp`, `coins`, `wins_crew`, `wins_impostor`, `games_impostor`, `tasks`, `reports`, `sabotages_fixed`, `correct_votes`, `times_ejected`. | `iu.top` |
| `/iu msg <player> <text>` | Private message. Aliases: `/iu w`, `/iu tell`. See [Chat](../features/chat.md#private-messages). | none |
| `/iu r <text>` | Reply. Alias: `/iu reply`. | none |
| `/iu streamer` | Streamer immunity on/off: never drawn as impostor. See [Streamer mode](../gameplay/streamer-mode.md). | `iu.streamer` |

## Arena setup: `/iu setup`

All need **`iu.admin.setup`**, must be run by a player and can't be used while you are in a match. The full list is in [Your first arena](../getting-started/first-arena.md#setup-commands); the ship map commands are in [Ship map](../gameplay/ship-map.md#commands).

| Command | Description |
|---|---|
| `/iu setup` | Arena list GUI: click one to edit it. |
| `/iu setup guide [stop\|done <step>]` | Step-by-step chat guide. |
| `/iu setup create <name>` / `edit <name>` | Start a session (opens the GUI). |
| `/iu setup gui` | Open the setup GUI (alias `menu`). |
| `/iu setup clone <arena> <new-name>` / `rename <new-name>` / `delete <name>` | Manage arenas. |
| `/iu setup list` / `tp [arena]` / `info` | List / teleport / summary. |
| `/iu setup world [folder]` | Template world. |
| `/iu setup min <n>` / `max <n>` / `impostors <n>` | Player limits and impostors. |
| `/iu setup spawn` / `clearspawns` | Spawns. |
| `/iu setup meeting` / `clearmeeting` | Meeting seats. |
| `/iu setup button` | Emergency button block (toggle). |
| `/iu setup task <id> [COMMON\|SHORT\|LONG] [minigame] [room]` / `removetask <id>` | Tasks and stations. |
| `/iu setup vent <id>` / `removevent <id>` | Vent networks. |
| `/iu setup panel <type>` / `clearpanels <type>` | Sabotage repair panels. |
| `/iu setup doors` / `cleardoors` | Doors for the doors sabotage. |
| `/iu setup lobby` | Arena's waiting lobby. |
| `/iu setup markers` | Show / hide map markers. |
| `/iu setup map [render\|room\|removeroom\|rooms\|corner1\|corner2\|top\|auto]` | Ship map. |
| `/iu setup save` / `cancel` | Save / discard the session. |

## Match and server management

| Command | Description | Permission |
|---|---|---|
| `/iu start [arena]` | Starts a match now. Alias: `/iu forcestart`. | `iu.admin.game` |
| `/iu stop [arena]` | Stops a match (no winner). | `iu.admin.game` |
| `/iu forcejoin <player> [arena]` | Puts a player into a match (the arena is optional with only one). Tells you if the join was refused. | `iu.admin.game` |
| `/iu kick <player>` | Removes a player from their match. | `iu.admin.game` |
| `/iu impostor <player> [arena]` | Toggles a player of a waiting lobby as a forced impostor. Alias: `/iu impostore`. | `iu.admin.match` |
| `/iu impostor list\|clear [arena]` | Shows / clears the forced impostors. | `iu.admin.match` |
| `/iu admin <tool> ...` | Match tools: see [Admin tools](admin-tools.md). | `iu.admin.match` |
| `/iu admin` (or `/iu admin build`) | Build mode in the hub (removes the hub items). | `iu.admin.build` |
| `/iu setlobby` | Sets the global hub where you stand. | `iu.admin.setup` |
| `/iu coins <give\|take\|set> <player> <amount>` | Coins. | `iu.admin.economy` |
| `/iu xp <give\|take\|set> <player> <amount>` | XP. | `iu.admin.economy` |
| `/iu npc create play` | Citizens NPC that opens the arena menu. | `iu.admin.npc` |
| `/iu npc create join <arena>` | Citizens NPC that joins an arena, with a live hologram. | `iu.admin.npc` |
| `/iu npc remove [id]` / `/iu npc list` | Removes (by id, or the NPC selected in Citizens) / lists linked NPCs. | `iu.admin.npc` |
| `/iu reload` | Reloads configuration, messages and arenas. | `iu.admin.reload` |
| `/iu debug` | Debug logging on/off until restart. | `iu.admin.debug` |
| `/iu debug report` | Writes a support report. | `iu.admin.debug` |
| `/iu resetstats <stats\|all>` | Resets every player's statistics, with confirmation and backup. See [Stats](../features/progression.md#resetting-the-statistics). | `iu.admin.resetstats` |

Commands with an optional `[arena]` use, when it's left out, the match you are in or watching, otherwise the only arena.

## Signs

Write `[IU]` on the first line and the arena name on the second (needs `iu.admin.signs`). See [Hub, NPCs & signs](../features/hub.md#join-signs).

## Commands during a match

While a player is in a match (or a queue), other commands are blocked (`commands.block-in-game` in `config.yml`), so nobody can teleport away or pass on information. Add exceptions to `commands.allowed`. Players with `iu.admin.bypass` are never blocked, and `/iu` itself can never be blocked.
