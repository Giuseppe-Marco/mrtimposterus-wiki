# Installation

This page gets MRTImposterUs running on your server: what you need, which files appear on the first start, how to update safely and how to pick a language and a database.

## Requirements

| What | Version |
|---|---|
| Server software | **Paper 1.21.4 – 1.21.11** or **Paper 26.1 / 26.2** |
| Java | **21** or newer |

The same jar works on both generations: on 26.x the plugin detects the new world layout and uses the matching API.

### Optional plugins

None of these are required. Install them only if you want what they add.

| Plugin | What it adds |
|---|---|
| **PlaceholderAPI** | `%imposterus_*%` placeholders for scoreboards, tab lists, holograms... See [Placeholders](../reference/placeholders.md). |
| **ProtocolLib** | Impostors glow through walls, visible **only to the other impostors** (`impostor-glow` in `game.yml`; without ProtocolLib that glow stays off, because a normal glow would reveal them to everyone). Dead and venting players also stay in the tab list. See [Anonymity & tab list](../gameplay/anonymity.md). |
| **ItemsAdder** | The knife resource pack is copied into it by itself (then run `/iazip`). See [Knife & resource pack](../gameplay/knife.md). |
| **Citizens** | Clickable arena NPCs with `/iu npc`, with a live hologram. See [Hub, NPCs & signs](../features/hub.md). |
| **TAB** | Tab list formatting with the plugin's placeholders (the plugin already draws its own sidebar). |
| **Multiverse-Core** | Handy for loading your map and lobby worlds. MRTImposterUs loads after it, so the arena templates already exist when the plugin starts. |

!!! tip "TAB plugin"
    The plugin draws its own sidebar scoreboard but does not format the tab list. If you use **TAB**, read the TAB notes in [Placeholders](../reference/placeholders.md#names-and-colours-with-the-tab-plugin) and the [FAQ](../faq.md): TAB's `scoreboard-teams` needs one condition so suit colours and hidden name tags keep working.

## Installing

1. Stop the server.
2. Put `MRTImposterUs-2.1.0.jar` in the `plugins/` folder.
3. Start the server. The plugin creates its folder `plugins/MRTImposterUs/` with all the default files.
4. Pick your language in `config.yml` (see [Language](#language) below).
5. Stand where your hub should be and run `/iu setlobby`.
6. Create your first arena: follow [Your first arena](first-arena.md).

!!! warning "Set the hub with `/iu setlobby`"
    Until you run `/iu setlobby`, the hub items and the hub protection are **not active anywhere**. Don't type a world into `lobby.yml` by hand unless it really is your hub: if you write your survival world there, everyone entering it becomes invulnerable and receives the compass.

When an admin joins and no arena exists yet, the plugin offers the step-by-step setup guide in chat (turn this off with `setup-guide.announce-on-join` in `config.yml`).

After editing files by hand, apply the changes with `/iu reload`.

## Files created on the first start

Everything lives in `plugins/MRTImposterUs/`:

```text
plugins/MRTImposterUs/
├── config.yml          core: language, debug, admin, scoreboard, tab, chat, commands, stats, resource pack...
├── lobby.yml           main hub and hub items
├── game.yml            match rules: players, impostors, kills, vision, anonymity, ghosts, knife, ship map, items, sounds
├── tasks.yml           tasks per player, task bar, markers, minigame tuning
├── sabotage.yml        sabotage cooldowns, panels, lights, communications, reactor, oxygen, doors
├── meeting.yml         discussion and voting, emergency button, reports, vote display
├── roles.yml           optional roles (sheriff, engineer...)
├── economy.yml         XP, coins, reward commands
├── languages/
│   ├── messages_en.yml
│   └── messages_it.yml
├── arenas/
│   └── Skeld.yml.example
├── resourcepack/
│   └── MRTImposterUs-pack.zip
└── stats.db            player statistics (SQLite)
```

Other folders appear when they are first needed: `maps/` (ship map pictures), `logs/` (match logs), `reports/` (debug reports), `backups/` (statistics backups).

The `Skeld.yml.example` file in `arenas/` is a fully commented example arena. It ends in `.example`, so it is **never loaded**: it is only documentation. Your own arenas are written in the same folder as `<Name>.yml` by the setup GUI.

Each file is described in [Configuration files](configuration.md).

## Updating

!!! danger "Never hot-reload the plugin"
    Do **not** update with `/reload` or plugin managers. Matches copy worlds, hold players and open database connections: always do a full restart. `/iu reload` only reloads the configuration, messages and arenas; it does not load a new jar.

1. **Stop** the server.
2. Optionally make a backup of `plugins/MRTImposterUs/`.
3. **Replace** the old jar in `plugins/` with the new one (delete the old file, don't keep both).
4. **Start** the server.

On every start the plugin adds any **new keys** to your files, without touching the values you changed.

!!! note "Upgrading from 1.x"
    The old `messages.yml` is moved to `languages/messages_it.yml` (set `language: it` to keep Italian), and the `stats` section of the old `stats.yml` moves back into `config.yml` with your values (MySQL credentials included). See the [Changelog](../changelog.md#200-new-technical-base) for the other 2.0 changes.

## Language

All texts (chat, GUIs, scoreboard, titles) live in `languages/messages_<code>.yml`, written in MiniMessage. Choose the file with `language` in `config.yml`:

```yaml
# Language of the plugin's texts. Available values: en, it.
language: en
```

- Bundled languages: `en` (English) and `it` (Italian).
- Apply the change with `/iu reload`.
- Keys missing from the selected file fall back to English.
- **Your own translation:** create `languages/messages_<code>.yml` (for example by copying `messages_en.yml`), translate it and set `language: <code>`.

## Storage: SQLite or MySQL

Statistics, levels, XP, coins and the streamer immunities are stored in a database, configured in the `stats` section of `config.yml`:

```yaml
stats:
  enabled: true
  # SQLITE (local stats.db file, no configuration) or MYSQL.
  storage: SQLITE
  leaderboard-refresh-seconds: 60
  # Only used if storage: MYSQL.
  mysql:
    host: localhost
    port: 3306
    database: imposterus
    user: root
    password: ''
```

=== "SQLite (default)"

    Nothing to do: the data is saved in `plugins/MRTImposterUs/stats.db`.

=== "MySQL"

    1. Create an empty database (for example `imposterus`) and a user that can use it.
    2. Set `storage: MYSQL` and fill in `host`, `port`, `database`, `user` and `password`.
    3. **Restart** the server.

    The MySQL driver must be provided by the server. Idle connections are re-opened automatically.

!!! note
    The `stats` section is read at startup only: after changing `enabled`, `storage` or the MySQL login, restart the server instead of using `/iu reload`.

Statistics are always global, never per arena. More in [Stats, levels & rewards](../features/progression.md).
