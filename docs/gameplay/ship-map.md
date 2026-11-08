# Ship map

At the start of the round everyone (ghosts too) gets a **vanilla map of the ship**: the arena seen from above, with the **room names**, **your position** and the **next station of each of your tasks**.

By default it sits in the **off hand**, so it's always visible in a corner of the screen while the main hand holds the other items.

## Settings (`game.yml`)

| Key | Default | Meaning |
|---|---|---|
| `ship-map` | `true` | Give the ship map. |
| `ship-map-tasks` | `true` | Show the stations of your tasks on the map. Hidden during the communications sabotage. Impostors see their fake tasks, like a crewmate. |
| `ship-map-offhand` | `true` | Map in the off hand. `false` = last hotbar slot. |

## Rendering the picture

The picture is made **once** by an admin and then never changes. **Until it's made, no map is given.**

1. Open a setup session (`/iu setup edit <arena>`).
2. Stand in the map world.
3. Run `/iu setup map render`.

Run `render` again after changing the map, the area or the height.

## Commands

All inside a setup session (permission `iu.admin.setup`):

| Command | What it does |
|---|---|
| `/iu setup map` | Shows the area, scan height, rooms and whether the picture is drawn. |
| `/iu setup map render` | Creates the picture (again after changing the map, the area or the height). |
| `/iu setup map room <name>` | Adds a room label where you stand. |
| `/iu setup map removeroom [name]` | Removes a room label (by name, or the nearest). |
| `/iu setup map rooms` | Lists every label: added by hand, or taken from the `room` of the tasks. |
| `/iu setup map corner1` / `corner2` | Sets the area by hand. Default: everything placed in the arena + 8 blocks. |
| `/iu setup map top` | Reads the map from just above your feet down (useful below a ceiling). Default: 3 blocks above the highest point placed. |
| `/iu setup map auto` | Area and height automatic again. |

Room labels come from the `room` given to tasks (`/iu setup task <id> <type> <minigame> [room]`) plus the ones added with `/iu setup map room`.

## Files

```text
plugins/MRTImposterUs/maps/<arena>.yml
plugins/MRTImposterUs/maps/<arena>.png
```

Renaming or cloning an arena keeps its ship map.
