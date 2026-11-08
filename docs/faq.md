# FAQ & troubleshooting

!!! tip "Still stuck?"
    Run `/iu debug report` and send the generated file (`plugins/MRTImposterUs/reports/`) to whoever helps you: it contains the server, plugins, arenas and configuration, with database credentials hidden. `/iu debug` turns on detailed console logging until restart. See [Admin tools](reference/admin-tools.md#debug).

## Installation and updates

### How do I update the plugin?

Stop the server, replace the jar (delete the old one), start the server. Never use `/reload` or plugin managers: `/iu reload` only reloads configuration, messages and arenas. New configuration keys are added on start without touching your values. See [Updating](getting-started/installation.md#updating).

### Do I need to download a PlaceholderAPI expansion?

No. The `imposterus` expansion is built in. Install PlaceholderAPI and restart: the console shows `Hooked into PlaceholderAPI`.

### Hub items or protection are active in the wrong world

They only work in the world set with `/iu setlobby`. Don't type a world into `lobby.yml` by hand unless it really is your hub. See [Hub](features/hub.md).

## Placeholders, TAB and scoreboard

### TAB: suit colours or hidden name tags don't work in matches

If TAB's `scoreboard-teams` feature is on, TAB blocks the plugin's teams (you'll see lines like *"Blocked attempt to add player ... into team iu..."* in `plugins/TAB/anti-override.log`). Tell TAB not to manage players while they're in a match, in `plugins/TAB/config.yml`:

```yaml
scoreboard-teams:
  enabled: true
  disable-condition: '%world%=disabledworld|%imposterus_in_match%=1'
```

Then `/tab reload`. Details in [Placeholders](reference/placeholders.md#names-and-colours-with-the-tab-plugin).

### Two sidebars fight each other / the scoreboard flickers

Two plugins are drawing a sidebar. Disable TAB's scoreboard, or set `scoreboard.enabled: false` in `config.yml`. See [Scoreboard](features/scoreboard.md).

### The meeting list in the tab footer doesn't show

`tab-player-list` in `meeting.yml` writes the tab footer during meetings. If another plugin (e.g. TAB) manages the tab list header and footer, turn one of them off. The **Players** item (`player-list-item`) shows the same list without touching the tab list. See [Meetings](gameplay/meetings.md#discussion).

### Impostors don't glow for each other

`impostor-glow` is off by default and needs **ProtocolLib**: without it the glow stays off, because a normal glow would be visible to everyone. Impostors still see the red particle crown above each other (`impostor-markers`). See [Anonymity](gameplay/anonymity.md#impostors-recognising-each-other).

## Matches and worlds

### The match doesn't start / the map isn't copied

- `world-template` in the arena file must be the world's **folder name** in the server directory, not a Multiverse alias. Fix it with `/iu setup world <folder>`.
- You can't use a match copy (`imp_*`) as a template: set up the arena in the real map world.
- Run `/save-all` after editing the map: the plugin copies the files on disk.
- Run `/iu setup info`: the arena must be playable (spawns, meeting seats, emergency button, tasks).

### Players in different arenas can't see each other in the hub

`tab.per-game-visibility` hides players of other matches. If several arenas share the global lobby, give each arena its own lobby (`/iu setup lobby`) or turn the option off.

### The tab list shows who died

Install **ProtocolLib**: with it, everyone in the round always stays listed, dead or venting. Without it, the players of a running round are taken off each other's tab list instead, so no name can vanish. See [Anonymity & tab list](gameplay/anonymity.md#tab-list).

### Nobody gets the ship map

The picture has to be made once: open a setup session, stand in the map world and run `/iu setup map render`. Until then no map is given. See [Ship map](gameplay/ship-map.md).

### A sabotage can't be started

The arena needs the panels of that sabotage (two for reactor and oxygen) and, for the doors sabotage, at least one door. Only one sabotage runs at a time, and impostors can't sabotage from a vent. See [Sabotages](gameplay/sabotages.md).

## Knife and resource pack

### Players see a purple-and-black cube in the impostor's hand

They don't have the plugin's resource pack. Make the pack **required** (with `resource-pack.enabled` + `required: true`, ItemsAdder's required pack, or your own server pack), or turn `game.knife` off. See [Knife & resource pack](gameplay/knife.md).

### I installed ItemsAdder but the knife doesn't show

Run `/iazip` after the plugin copied the knife into `plugins/ItemsAdder/contents/mrtimposterus/` (and again after updates), and make ItemsAdder's pack required.

## Statistics

### Can I reset everyone's statistics?

Yes, with `/iu resetstats <stats|all>`: it asks for a confirmation code, is refused while a match is running and always takes a backup first. It needs `iu.admin.resetstats`, which is not part of `iu.admin`. See [Stats](features/progression.md#resetting-the-statistics).

### Can players spend their coins?

Not in MRTImposterUs: it has no shop. Coins are tracked for other plugins, and you can use [reward commands](features/progression.md#reward-commands) to hook up your economy.
