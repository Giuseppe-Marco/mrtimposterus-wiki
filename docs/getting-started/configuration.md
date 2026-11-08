# Configuration files

MRTImposterUs splits its settings into several files in `plugins/MRTImposterUs/`. They are all merged at runtime into a single configuration view. After editing a file by hand, apply the changes with `/iu reload`.

| File | What's inside |
|---|---|
| `config.yml` | Core: `language`, `debug`, `admin` (match log), sidebar `scoreboard`, `tab` (per-match visibility), `chat`, `commands` (blocked during a match), `messages` (small caps, command label), `stats` (SQLite/MySQL), `resource-pack`, `update-checker`, `metrics`, `setup-guide`. |
| `lobby.yml` | Main hub: position (set with `/iu setlobby`), teleport on join, protection, void level, hub items and their slots. |
| `game.yml` | Match rules: players, countdown, reveal, impostors, kill cooldown and distance, vision, impostor markers and glow, hidden name tags and skins, anonymous names, ship map, ghosts, knife, rejoin, items, sounds. |
| `tasks.yml` | Tasks per player (common / long / short), task bar, task markers, minigame tuning. |
| `sabotage.yml` | Sabotage cooldowns, repair panels, lights, communications, reactor, oxygen and doors. |
| `meeting.yml` | Discussion and voting time, emergency meetings per player and cooldown, report distance, role reveal on ejection, vote display, Players item and tab list. |
| `roles.yml` | Optional roles (sheriff, engineer; medic and scientist are planned and not implemented yet). |
| `economy.yml` | `progression` (XP and coins, XP per level – global only) and `rewards` (console commands for winners and kills). |
| `languages/messages_<code>.yml` | Every text, in MiniMessage. See [Language](installation.md#language). |
| `arenas/*.yml` | One file per arena, written by the setup GUI. See [Your first arena](first-arena.md). |

The `game`, `tasks`, `sabotage`, `meeting`, `roles`, `sounds`, `rewards` and `commands` sections can be changed **per arena**: see [Per-arena settings](arena-settings.md).

!!! note "A YAML error is reported by name"
    If a file has a YAML error, the console tells you which file it is, and the file is never overwritten.

## config.yml

| Key | Default | Meaning |
|---|---|---|
| `language` | `en` | `en` or `it` (or your own `messages_<code>.yml`). |
| `debug` | `false` | Logs the whole internal flow to the console. Keep `false` in production (`/iu debug` toggles it until restart). |
| `admin.match-log` | `true` | Writes a timeline of every match to `logs/<arena>-<date>.txt`. See [Admin tools](../reference/admin-tools.md#match-log). |
| `scoreboard.*` | | The plugin's sidebar. See [Scoreboard](../features/scoreboard.md). |
| `tab.per-game-visibility` | `true` | Each player only sees the players of their own match; players not in a match share the lobby group. Ghosts stay invisible to the living in any case. |
| `chat.*` | | Chat scope, formats, free-play chat mode, prefixes, private messages, moderation. See [Chat](../features/chat.md). |
| `commands.block-in-game` | `true` | Blocks other commands while in a match or queue (also namespaced forms like `essentials:spawn`). `iu.admin.bypass` is exempt; `/iu` can never be blocked. |
| `commands.allowed` | `[]` | Commands still allowed to players in a match. |
| `messages.smallcaps` | `true` | Renders all texts in small caps (tags, hex colours and placeholder values are left unchanged). |
| `messages.command-label` | `auto` | The command shown in texts: `auto` (`/iu`, or `/imposterus` when another plugin owns `/iu`), `imposterus` or `iu`. |
| `stats.*` | | Database. See [Storage](installation.md#storage-sqlite-or-mysql). |
| `resource-pack.*` | | Knife pack sending and ItemsAdder copy. See [Knife & resource pack](../gameplay/knife.md). |
| `update-checker.enabled` / `notify-admins` / `include-beta` | `true` / `true` / `false` | Modrinth update check. See [Admin tools](../reference/admin-tools.md#update-checker). |
| `metrics` | `true` | Anonymous bStats usage statistics. |
| `setup-guide.announce-on-join` | `true` | Offer the setup guide to admins on join while no arena exists. |

## lobby.yml

| Key | Default | Meaning |
|---|---|---|
| `lobby.world`, `x`, `y`, `z`, `yaw`, `pitch` | empty | The hub. Set it with `/iu setlobby`, don't type it by hand. |
| `lobby.spawn-on-join` | `true` | Teleports anyone joining the server to the hub (players rejoining a running match stay where they were). |
| `lobby.protect` | `true` | No breaking, placing, damage, hunger, item drops or PvP in the hub. `iu.admin.build` is exempt only in build mode (`/iu admin`). |
| `lobby.void-level` | `-100` | Below this Y a hub player is sent back to the spawn. |
| `lobby.hub-items` | `true` | Gives the hub items (arena menu, stats) in the hub world, and removes them when leaving it. |
| `lobby.clear-inventory-in-hub` | `false` | Clears the inventory when the hub items are given. Only for a dedicated hub server. |
| `lobby.items.stats-slot` | `7` | Hotbar slot of the stats item. |
| `lobby.compass.enabled` / `give-on-join` / `slot` | `true` / `true` / `4` | The compass that opens the arena menu. |

`protect`, `void-level` and `hub-items` work **only** in the world set with `/iu setlobby`. More in [Hub, NPCs & signs](../features/hub.md).

## game.yml

| Key | Default | Meaning |
|---|---|---|
| `min-players` / `max-players` | `4` / `12` | Used when the arena doesn't set its own `players.min` / `players.max`. |
| `countdown-seconds` | `30` | Countdown once `min-players` is reached. |
| `reveal-seconds` | `5` | How long the role reveal stays on screen (players are frozen). |
| `auto-start` | `true` | Start the countdown automatically. |
| `end-delay-seconds` | `10` | End screen before the arena is reset. |
| `impostors` | `4: 1`, `7: 2`, `11: 3` | Impostors per number of players (the highest threshold reached applies; never more than parity). Ignored if the arena sets a fixed `players.impostors`. |
| `kill-cooldown-seconds` | `30` | Wait between kills. |
| `first-kill-cooldown-seconds` | `20` | Cooldown at the start of the match. |
| `kill-item-delay-seconds` | `2` | Seconds after the spawn before impostors get the kill item (`0` = at once). |
| `kill-distance` | `3.0` | Maximum kill distance in blocks. |
| `impostors-can-kill-each-other` | `false` | |
| `impostor-night-vision` | `true` | Impostors see during the lights sabotage. |
| `show-vent-markers` | `true` | Particles on vents, for those who can use them. |
| `impostor-markers` / `impostor-marker-range` | `true` / `32.0` | A crown of red particles above accomplices, visible only to other impostors. |
| `impostor-glow` | `false` | Impostors glow through walls for other impostors (needs ProtocolLib). |
| `hide-nametags` | `true` | Names above heads hidden once the match starts. |
| `anonymous-names` | `true` | Players go by their suit colour everywhere. See [Anonymity](../gameplay/anonymity.md). |
| `colored-names` | `false` | Only with `anonymous-names: false`: names in suit colour in match chat and vote menu. |
| `ship-map` / `ship-map-tasks` / `ship-map-offhand` | `true` / `true` / `true` | See [Ship map](../gameplay/ship-map.md). |
| `ghost-haunt` / `ghost-see-roles` | `true` / `false` | See [Ghosts](../gameplay/ghosts.md). |
| `disguise-impostor-items` | `true` | With hidden skins, impostor items look like harmless objects. |
| `knife` / `knife-visible` / `kill-animation` | `false` / `false` / `true` | See [Knife & resource pack](../gameplay/knife.md). |
| `show-corpse-names` | `false` | Victim's name above the corpse. |
| `hide-skins` | `true` | Players become invisible, only the coloured leather armour is seen. |
| `world-time` | `6000` | Fixed time of the match world. |
| `block-teleport-items` | `true` | Blocks ender pearls and chorus fruit in a match. |
| `rejoin-grace-seconds` | `45` | Time to rejoin after a disconnect (`0` = eliminated at once). |
| `quick-join` | `true` | `/iu join` without an arena joins the fullest available match; `false` opens the menu. |

The `items` section sets the material of every item (`tasks`, `report`, `player-list`, `kill`, `sabotage`, `kill-disguised`, `sabotage-disguised`, `haunt`, `compass`, `stats`, `color`, `leave`). Items are recognised by an internal tag, never by their material, so changing them breaks nothing. A wrong material name gives a console warning and the default is used.

The `sounds` section has `enabled` (`true`) and `master-volume` (`0.0`–`1.0`) for the plugin's own sound effects.

## tasks.yml

| Key | Default | Meaning |
|---|---|---|
| `common` / `long` / `short` | `1` / `2` / `3` | Tasks of each type per player. |
| `show-bossbar` | `true` | Global task bar. |
| `show-markers` / `marker-radius` | `true` / `24.0` | Beam and ring on the next station of your tasks, visible only to you. |
| `markers-for-impostors` | `false` | Show markers to impostors for their fake tasks too. |
| `glow-markers` | `true` | Glowing outline of the station through walls. |
| `minigames.*` | | Tuning of each minigame. See [Tasks](../gameplay/tasks.md#minigame-tuning). |

## sabotage.yml

| Key | Default | Meaning |
|---|---|---|
| `initial-cooldown-seconds` | `10` | Before the first sabotage is available. |
| `cooldown-seconds` | `30` | Before the next one, counted from the repair. |
| `hold-radius` | `3.0` | How far you may move from a panel you're holding. |
| `glow-panels` | `true` | During a critical sabotage the panels glow red through walls. |
| `lights.*`, `communications.*`, `reactor.*`, `oxygen.*`, `doors.*` | | Per sabotage. See [Sabotages](../gameplay/sabotages.md). |

## meeting.yml

| Key | Default | Meaning |
|---|---|---|
| `discussion-seconds` | `30` | Discussion only, nobody can vote yet. |
| `voting-seconds` | `45` | Voting; closes earlier if everyone votes. |
| `emergency-per-player` | `3` | Emergency meetings per player for the whole match. |
| `emergency-cooldown-seconds` | `15` | Button cooldown at the start and after every meeting (`0` = off). |
| `report-distance` | `4.0` | Maximum distance to report a body. |
| `button-markers` | `true` | Red halo on the emergency button (grey smoke while blocked by a sabotage). |
| `reveal-role` | `true` | Reveal the role of whoever is ejected. |
| `show-votes` | `true` | Show the vote count at the end. |
| `announce-vote-cast` | `true` | Announce that a player has voted (not for whom). |
| `live-votes` | `true` | Live counts on the ballot; `false` = blind voting. |
| `player-list-item` / `tab-player-list` | `true` / `true` | Players item and tab footer list during meetings. |

Details in [Meetings & voting](../gameplay/meetings.md).

## roles.yml

| Role | Keys | What it does |
|---|---|---|
| Sheriff | `enabled: false`, `chance: 1.0` | A single shot: if they shoot a crewmate, they die instead. |
| Engineer | `enabled: false`, `chance: 1.0`, `can-vent: true` | Can use vents like the impostors. |
| Medic | `enabled: false`, `chance: 1.0` | **Not implemented yet**: if enabled, it is assigned and announced but plays as a normal crewmate. |
| Scientist | `enabled: false`, `chance: 1.0` | **Not implemented yet**, like the medic. |

All roles are on the crew side and count as normal crewmates for the win conditions. `chance` (0.0–1.0) is the probability the role appears in a match when enabled. Enable one at a time so as not to unbalance the match.

## economy.yml

See [Stats, levels & rewards](../features/progression.md).
