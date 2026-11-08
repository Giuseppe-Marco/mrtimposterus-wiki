# Changelog

Full version history: [Modrinth versions](https://modrinth.com/plugin/mrtimposterus/versions).

Always update with a full restart (no hot reload): configuration files are upgraded automatically. See [Updating](getting-started/installation.md#updating).

## 2.1.0

### ⭐ Highlights
- **Knife and kill move.** The kill item can be a small knife (`game.knife`): only its holder sees it, the others see an empty hand (`knife-visible: true` shows it to everyone). Every kill plays a quick knife slash seen only by the killer and the victim (`game.kill-animation`); nobody is frozen, the body and the cooldown happen at once.
- **Resource pack, ready to use.** The pack with the knife is written to `plugins/MRTImposterUs/resourcepack/`. The plugin can send it to players (`resource-pack` in `config.yml`), or, with **ItemsAdder**, copies it by itself into `contents/mrtimposterus` (then run `/iazip`). A customised pack is never overwritten.
- **Ship map.** Everyone gets a vanilla map of the ship in the off hand: room names, your position and the next station of each of your tasks (hidden during communications). It is drawn once with `/iu setup map render` and never redrawn.
- **Who is alive, during the discussion.** A "Players" item, given only while a meeting runs, opens the ballot read-only from the very start of the discussion; the tab list footer shows alive and dead for the whole meeting (`player-list-item`, `tab-player-list` in `meeting.yml`).
- **Streamer mode.** `/iu streamer` (permission `iu.streamer`): an on/off switch; while on, the player is never drawn as impostor. Saved in the stats database (table `iu_streamers`), untouched by a stats reset.
- **Anonymous names** (`game.anonymous-names: true`): once the round starts players go by their suit colour everywhere — chat, vote menu, reports, ejections, kill and disconnect messages.
- **Admin tools** (`/iu admin ...`, permission `iu.admin.match`, also from the console): admin panel GUI, full match and player pages, live staff alerts, `tp`, `meeting`, `endvote`, `sabotage <type|fix>`, `kill`/`revive`, `end <crew|impostors|draw>`, `pause`, `settime`, `role`, `task <complete|reset>`, `broadcast`, `mute`.
- **Match log** (`admin.match-log: true`): every match is written to `logs/<arena>-<date>.txt` with roles, kills, reports, votes, ejections, sabotages and admin actions.

### Gameplay
- **Tab list**: with ProtocolLib everyone in the round always stays listed, dead or venting, so the tab list no longer gives deaths or vents away. Without ProtocolLib the players of a running round are off each other's tab list instead.
- Left-click kills work (as well as right-click).
- The ballot can be closed to type in chat and reopened with the Players item; no vote by the end counts as a skip.
- With `live-votes: false` the ballot really hides the counts until the result.
- `game.colored-names` (default `false`): with anonymous names off, paints names in the suit colour in the match chat and the vote menu.
- The meeting scoreboard shows your own colour.
- The coloured suit is put on only when the round starts: in the waiting lobby everyone keeps their own skin.
- Everyone spawns holding the task list; impostors get the kill item `game.kill-item-delay-seconds` (default 2) after the spawn.
- The vote menu stays open after voting, so you can watch the other votes come in.
- Inside a vent the camera turns freely (only walking is locked), and so it does at the meeting table.
- A rejoin during a meeting lands on the player's own seat.
- The sheriff's shot is no longer counted as an impostor kill (no kill stats, rewards or cooldown) and the spent weapon is taken back.
- Task markers are hidden during meetings.
- `/iu debug` now also logs vents, tab list, streamer immunity, the Players item, the kill move and the resource pack.

### Fixes
- An impostor could kill from inside a vent.
- The tab list showed who had died or entered a vent.
- A ghost who disconnected could shrink the task bar and hand the crew an instant win.
- Ghosts' visual tasks (medbay scan, garbage), the haunt menu (an impostor always read 0 tasks) and dead players at a repair panel leaked information.
- Lights or communications could become impossible to repair after a kick with the panel open.
- A leave or disconnect during the vote ended the match as "too many innocents ejected".
- Ghosts stayed grounded, and venting players hidden, when a match ended during a meeting.
- A doors sabotage could be started on an arena with no doors.
- Pressure plates, tripwires and farmland reacted to players in a match.
- Players could get stuck inside a full-block vent when climbing out.
- An impostor could report a body by clicking it with the kill item.
- Name tags stayed visible (in suit colour) while the match map was loading.
- **Arena files**: an arena file with a YAML error is no longer loaded as an empty arena; it is reported and never overwritten. Arenas are saved atomically.
- Task or vent ids with a dot broke the arena file; a mistyped minigame name was accepted at setup.
- Renaming or cloning an arena lost its ship map and its NPC links.
- The **sheriff** never received the weapon, so the role could not shoot.
- Medbay scan: several scans could be stacked; an impostor's scan is visible only to them.
- The download task window stayed open after completing, and its items could be taken.
- Ghosts could open and close doors.
- Relogging during a lights sabotage removed the darkness; the task bar is shown again after a rejoin.
- `game.hide-skins` / `impostor-night-vision` set per arena were partly ignored.
- A player timing out while the map was still loading could hand the crew a recorded win.
- Players in a vent could attach their spectator camera to other players.
- A vote for a player who died or left during the vote no longer ejects a ghost.
- `/iu coins|xp` with negative or huge amounts, `/iu forcejoin` reporting success on a refused join, `/iu resetstats` spending the code on a refusal or being undone by a player joining at that moment.
- With no hub set, joining the server no longer teleports everyone to the main world spawn.
- Outside a match the sidebar no longer replaces another plugin's scoreboard.
- PlaceholderAPI values in the chat format are escaped (no click/hover injection through nicknames).
- One failed inventory restore no longer leaves the whole arena stuck in reset.
- A YAML error in a topic file (game.yml, ...) is now reported by name.

### Build
- The jar is no longer obfuscated: real class names in stack traces.

---

## 2.0.0 — new technical base

MRTImposterUs now runs on the same technical base as MRTBattleRoyale 2.x: same
configuration layout, same languages, same statistics engine, same admin tools.

### Breaking changes

- **Texts in English and Italian.** Messages are split into `languages/messages_en.yml`
  and `languages/messages_it.yml`; pick one with `language: en|it` in `config.yml`
  (default `en`). An existing `messages.yml` is moved to `languages/messages_it.yml`
  automatically — set `language: it` to keep Italian.
- **`stats.yml` is gone**: its `stats` section moves back into `config.yml` on the first
  start, with your values (MySQL credentials included).
- **Granular permissions**: `iu.play`, `iu.stats`, `iu.stats.others`, `iu.top`,
  `iu.admin.setup`, `iu.admin.game`, `iu.admin.match`, `iu.admin.build`,
  `iu.admin.economy`, `iu.admin.npc`, `iu.admin.signs`, `iu.admin.reload`,
  `iu.admin.debug`, `iu.admin.bypass`, `iu.admin.notify`, `iu.admin.resetstats`,
  `iu.chat.bypass`, grouped under `iu.player`, `iu.admin` and `iu.*`
  (see [Permissions](reference/permissions.md)).
- **Chat scope**: by default (`chat.scope: iu`) the plugin now handles only the
  ImposterUs chats (hub and matches) and leaves the rest of the server's chat alone.
  Set `chat.scope: server` for the old "whole server" behaviour.
- PlaceholderAPI yes/no values follow the language (`placeholder.bool-true` /
  `placeholder.bool-false`): `Yes`/`No` in English, `Sì`/`No` in Italian. Update TAB
  conditions that compared them with `Si`.

### New

- **Built-in sidebar scoreboard**, one board per situation: hub, waiting, countdown,
  match, meeting, ghost, ending (`scoreboard` in `config.yml`, texts in
  `scoreboard.boards`). The board is personal: your role and kill cooldown are shown
  only to you; task progress is hidden while communications are sabotaged. Name colours,
  hidden name tags and glow keep working (the main scoreboard teams are copied).
  See [Scoreboard](features/scoreboard.md).
- **`/iu resetstats <stats|all>`**: resets every player's statistics, with a
  confirmation code, refused during a match, and an automatic backup first
  (`backups/stats-<date>.db` for SQLite, `iu_stats_backup_<date>` table for MySQL).
- **Setup guide** in chat (`/iu setup guide`), offered automatically to admins while no
  arena exists; **map markers** visible only to the editor (`/iu setup markers`); arena
  list GUI (`/iu setup`).
- **`/iu admin spectate [arena]`**: watch a running match invisibly.
- **Update checker** (Modrinth), **bStats** metrics (`metrics`), **`/iu debug report`**.
- New placeholders: `in_match`, `name_hex`, `name_color_hex`, `name_color`,
  `player_count`; leaderboard categories `games_impostor`, `reports`,
  `sabotages_fixed`, `times_ejected` (plus aliases `level`, `crew`, `impostor`,
  `votes`, `ejected`).
- New config keys: `chat.scope`, `chat.hub-worlds`, `chat.default-name-color`,
  `scoreboard.*`, `update-checker.*`, `metrics`, `setup-guide.announce-on-join`,
  `messages.command-label`.

### Fixes and improvements

- Statistics: every database read and write now runs on a single ordered thread
  (no more older saves landing after newer ones), pending writes are flushed on shutdown,
  MySQL reconnects after idle timeouts and logs in on MySQL 8, a failed read never
  caches (and later saves) a blank profile, and profiles of players already online are
  loaded after a hot reload.
- Statistics: counters can no longer overflow; the leaderboard value shown by
  `/iu top` now matches the category (deaths used to show XP); ties are ordered by name.
- Placeholders requested off the main thread (TAB) are answered from a snapshot instead
  of reading the live match.
- Chat: hub/match routing runs on the main thread; formats translate legacy colour
  codes from PlaceholderAPI; player text with `allow-player-colors` keeps colours but
  never click/hover tags; match chat is logged to the console with the arena name; a
  chat line during shutdown is dropped instead of throwing.
- Plus the fixes of the shared base: safer world copies on Paper 1.21.x and 26.x, config
  files with a YAML error are no longer overwritten, safer shutdown.

---

## 1.0.0 — first release

### Game

- Full Among Us-style matches: secret roles, tasks, sabotages, kills, bodies, meetings
  and votes
- **14 task minigames**, each with its own interface
- **5 sabotages**: lights, communications, reactor, oxygen, doors. Reactor and oxygen
  have a countdown that loses the match
- **4 roles**: crewmate, impostor, and the optional sheriff and engineer (medic and
  scientist planned, still off)
- Vent networks, with markers visible only to those who may use them
- Playable ghosts: they keep doing tasks and can haunt the living
- Meetings with assigned seats, live votes and ejection
- Multi-block emergency button, with limited uses and cooldown

### Administration

- Full arena setup from a GUI
- Arena cloning and renaming
- Clickable Citizens NPCs with a live status hologram
- `[IU]` join signs
- Statistics on SQLite or MySQL, with levels, XP and coins
- Reward commands at the end of the match
- PlaceholderAPI placeholders, plus the `top_<category>_<n>_name|value` leaderboards
- Built-in chat: per-match channels, moderation, private messages
- Hub protection and anti-void

### Compatibility

- Paper 1.21 to 26.x from one jar
- Works without PlaceholderAPI, Citizens and ProtocolLib
