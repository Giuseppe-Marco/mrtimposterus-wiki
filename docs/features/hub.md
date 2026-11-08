# Hub, NPCs & signs

## The hub

The main hub is where players arrive and return after a match. Set it where you stand with:

```text
/iu setlobby
```

(`/iu setup lobby` is different: it sets one arena's own waiting lobby.)

!!! warning "Nothing works until the hub is set"
    Until you run `/iu setlobby`, `lobby.world` stays empty and the hub items and protection are **not active anywhere**. With no hub set, joining the server doesn't teleport anyone. Don't type a world by hand unless it really is the hub.

### Settings (`lobby.yml`)

| Key | Default | Meaning |
|---|---|---|
| `spawn-on-join` | `true` | Teleports anyone joining the server to the hub. Players rejoining a running match (within `rejoin-grace-seconds`) stay in their round. |
| `protect` | `true` | No breaking, placing, damage, hunger, item drops or PvP in the hub. |
| `void-level` | `-100` | Below this Y a hub player is sent back to the spawn. |
| `hub-items` | `true` | Gives the hub items in the hub world. Leaving the hub world removes them again. |
| `clear-inventory-in-hub` | `false` | Clears the inventory when the hub items are given. Enable it only on a dedicated hub; otherwise only the old hub items are replaced. |
| `items.stats-slot` | `7` | Hotbar slot of the stats item. |
| `compass.enabled` / `give-on-join` / `slot` | `true` / `true` / `4` | The compass that opens the arena menu. |

`protect`, `void-level` and `hub-items` only work in the world set with `/iu setlobby`. If a slot is taken by one of the player's items, that item is moved to a free slot, never deleted.

### Hub items

| Item | Default material | What it does |
|---|---|---|
| Compass | `COMPASS` | Opens the arena menu. |
| Stats | `BOOK` | Your statistics (needs `iu.stats`). |

Materials are changed in `game.yml` (`items` section), names in the messages file (`lobby` section).

### Build mode

`/iu admin` (or `/iu admin build`) toggles **build mode** for staff with `iu.admin.build`: the hub items are removed and you can build in the protected hub. It never clears your inventory. Run it again to turn it off.

## Arena menu

`/iu menu` or the hub compass opens the **arena selector**: click an arena to join it. `/iu list` lists the arenas with their state in chat.

`/iu join` without an arena: with a single arena, joins it; otherwise joins the fullest available match (`quick-join: true` in `game.yml`) or opens the menu (`quick-join: false`).

## Join signs

1. Place a sign.
2. First line: `[IU]` (not case-sensitive). Second line: the arena name.

Creating one needs `iu.admin.signs`; without it the first line is cleared and it is just a normal sign. Clicking a join sign is the same as `/iu join <arena>`. Clicks work even if a protection plugin denies interaction in the hub.

## Citizens NPCs

With **Citizens** installed, staff with `iu.admin.npc` can place clickable NPCs where they stand:

| Command | What it does |
|---|---|
| `/iu npc create play` | NPC that opens the arena menu. |
| `/iu npc create join <arena>` | NPC that joins an arena, with a live status hologram above its head. |
| `/iu npc remove [id]` | Removes a linked NPC by id, or the NPC you have selected in Citizens. |
| `/iu npc list` | Lists the linked NPCs. |

Without Citizens, `/iu npc` tells you Citizens is missing. Renaming or cloning an arena keeps its NPC links.

## Players in the hub

With `tab.per-game-visibility: true` (`config.yml`) players in a match only see their own match, and players not in a match share the lobby group. If several arenas share the global lobby as waiting area, players of different arenas can't see each other there: give each arena its own lobby (`/iu setup lobby`) or turn the option off.
