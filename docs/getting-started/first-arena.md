# Your first arena

An **arena** is a map (the ship) plus everything the plugin needs to play on it: spawns, the meeting table, the emergency button, tasks, vents, sabotage panels and doors. Everything is set up in game, with no file editing: with the **chat guide**, the **setup GUI** or **commands** (you can mix them).

All setup commands need the `iu.admin.setup` permission and can't be used while you are inside a match.

## 1. Prepare the map world

1. Build (or download) your map in **its own world**.
2. Make sure that world is loaded on the server (for example with Multiverse-Core), so you can walk in it.
3. Teleport into it.

This world is the arena's **template**. Every match is played on a **temporary copy** of it, so doors and panels are back in place every round and the original world is never modified.

!!! warning "Use the real map world"
    You can't use a match copy (`imp_*`) as a template: set up the arena in the real map world.

!!! tip "What is the template name?"
    The arena stores the world's **folder name** in the server directory, not a Multiverse alias. When you create the arena it takes the world you're standing in; you can change it later with `/iu setup world <folder>`.

## 2. Set it up

=== "Chat guide (easiest)"

    Run:

    ```text
    /iu setup guide
    ```

    The plugin tells you step by step what to do next, with **clickable** commands in chat. It follows the real state of the arena, so you can mix it with the GUI or with commands. The steps are:

    1. **World** – the template world.
    2. **Spawns** – where players appear at the start of the round.
    3. **Meeting seats** – the places around the meeting table.
    4. **Emergency button**.
    5. **Tasks** – and their stations.
    6. **Vents**.
    7. **Sabotage panels**.
    8. **Doors** – the ones the doors sabotage seals.
    9. **Lobby** – an optional waiting lobby for this arena.
    10. **Players** – limits and impostors.

    Stop it with `/iu setup guide stop`. Admins are offered the guide automatically on join while no arena exists (`setup-guide.announce-on-join` in `config.yml`).

=== "Setup GUI"

    1. Stand in the map world and run `/iu setup create <Name>`. The GUI opens (open it again any time with `/iu setup gui`; `/iu setup` alone lists the arenas, click one to edit it).
    2. Fill in the categories: players and impostors, template world, spawns, tasks and their stations, vents, sabotage panels, doors, meeting seats, emergency button, optional waiting lobby.
    3. Save (or run `/iu setup save`).

=== "Commands"

    Every step of the GUI can also be done with commands while a setup session is open. See [Setup commands](#setup-commands) below.

### When is an arena playable?

The arena is **playable** when it has:

- at least one **spawn**,
- at least one **meeting seat**,
- an **emergency button**,
- at least one **task with a station**.

You can save an unfinished arena: it simply stays out of play until it's complete. `/iu setup info` shows what is still missing.

### Markers in the map

While you edit an arena, its points (button blocks, task stations, vents, panels, doors) are shown in the map world as glowing markers with a name tag. Only you see them and they are never saved into the map. Turn them off/on with `/iu setup markers` or the button in the setup GUI.

## 3. Placing the points

| What | How |
|---|---|
| **Spawns** | Stand on the spot and run `/iu setup spawn`. `/iu setup clearspawns` removes them all. |
| **Meeting seats** | Stand on each seat around the table and run `/iu setup meeting`. Everyone gets their own seat at a meeting. `/iu setup clearmeeting` removes them. |
| **Emergency button** | Look at a block and run `/iu setup button`: it is added to the button (run it again on the same block to remove it). A button can be made of several blocks. |
| **Tasks** | `/iu setup task <id> [COMMON\|SHORT\|LONG] [minigame] [room]`, then **click the blocks** of its stations. Without a type the task is `SHORT`, without a minigame it is `wiring`. The `room` is optional and also labels the [ship map](../gameplay/ship-map.md). See [Tasks](../gameplay/tasks.md) for the minigame ids. |
| **Vents** | `/iu setup vent <id>` creates a vent network, then **click the vents** that belong to it. See [Vents](../gameplay/vents.md). |
| **Sabotage panels** | Look at a block and run `/iu setup panel <lights\|communications\|reactor\|oxygen\|doors>`. Reactor and oxygen need **two** panels. See [Sabotages](../gameplay/sabotages.md). |
| **Doors** | `/iu setup doors` turns door picking on: click the doors (doors, trapdoors or gates) to lock, click again to remove them. Build them **already closed**: the plugin opens them all at the start of every match. |
| **Waiting lobby** | `/iu setup lobby` sets this arena's own waiting lobby where you stand (optional). |
| **Players** | `/iu setup min <n>`, `/iu setup max <n>`, `/iu setup impostors <n>`. |

!!! tip "Put visual tasks in plain sight"
    The **medbay scan** and **empty garbage** tasks are *visual*: other players see you do them, so finishing one in front of a witness proves you're innocent. Place them where they can be seen.

## 4. The ship map

At the start of the round everyone gets a map of the ship in the off hand. The picture is made **once** by you: open a setup session, stand in the map world and run:

```text
/iu setup map render
```

Until then no map is given. All the options are in [Ship map](../gameplay/ship-map.md).

## 5. Save and test

Run `/iu setup save` (or the save button in the GUI). The arena is written to `arenas/<Name>.yml`. Join it with `/iu join <Name>`; staff can start it right away with `/iu start <Name>` instead of waiting for `min-players`.

## Setup commands

All of them need `iu.admin.setup` and a player (not the console).

| Command | What it does |
|---|---|
| `/iu setup` | List of arenas: click one to edit it. |
| `/iu setup help` | Usage line. |
| `/iu setup guide [stop\|done <step>]` | Chat guide. Steps: `world`, `spawns`, `meeting`, `button`, `tasks`, `vents`, `sabotage`, `doors`, `lobby`, `players`. |
| `/iu setup create <name>` | Start a session for a new arena in your current world (opens the GUI). |
| `/iu setup edit <name>` | Start a session on an existing arena (opens the GUI). |
| `/iu setup gui` | Open the GUI of the current session. Alias: `/iu setup menu`. |
| `/iu setup clone <arena> <new-name>` | Copy an arena. |
| `/iu setup rename <new-name>` | Rename the arena being edited. |
| `/iu setup delete <name>` | Delete an arena. |
| `/iu setup list` | List the arenas. |
| `/iu setup tp [arena]` | Teleport to an arena's map world. |
| `/iu setup info` | Summary of the arena being edited, with what is still missing. |
| `/iu setup world [folder]` | Use a different template world (folder name, not a Multiverse alias). Without a folder: the world you're in. |
| `/iu setup min <n>` / `max <n>` / `impostors <n>` | Player limits and impostors. |
| `/iu setup spawn` / `clearspawns` | Add a spawn here / remove all. |
| `/iu setup meeting` / `clearmeeting` | Add a meeting seat here / remove all. |
| `/iu setup button` | Add/remove the block you look at as emergency button. |
| `/iu setup task <id> [COMMON\|SHORT\|LONG] [minigame] [room]` | Create a task, then click the blocks of its stations. |
| `/iu setup removetask <id>` | Remove a task. |
| `/iu setup vent <id>` | Create a vent network, then click the linked vents. |
| `/iu setup removevent <id>` | Remove a vent network. |
| `/iu setup panel <type>` / `clearpanels <type>` | Add the block you look at as repair panel / remove the panels of a type. |
| `/iu setup doors` / `cleardoors` | Pick the doors the doors sabotage seals / remove all. |
| `/iu setup lobby` | Dedicated waiting lobby. |
| `/iu setup markers` | Show / hide the map markers. |
| `/iu setup map ...` | Ship map, see [Ship map](../gameplay/ship-map.md). |
| `/iu setup save` / `cancel` | Save / discard the session. |

## Writing an arena file by hand

Prefer files? Copy `arenas/Skeld.yml.example`, rename it to `<Name>.yml` (the name must match the file name), put your own coordinates and template world in, and run `/iu reload`. Coordinates in arena files always refer to the **template** world.

An arena file with a YAML error is reported in the console and is never loaded as an empty arena or overwritten.

Next: [Per-arena settings](arena-settings.md) to give this arena its own rules.
