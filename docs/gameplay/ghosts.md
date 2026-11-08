# Ghosts

Killed and ejected players don't leave the match: they become **ghosts**.

## What ghosts can do

| | Ghosts |
|---|---|
| **Fly** | Yes (not during meetings, when everyone stays seated) |
| **Seen by the living** | No, they are invisible to the living |
| **See in the dark** | Yes |
| **Tasks** | They keep doing their tasks, which **still count** for the crew's victory |
| **Chat** | Their own channel (`dead-prefix`), in every phase, that the living never hear |
| **Haunt** | With `ghost-haunt: true`, the **Haunt** item (`SPYGLASS`) lists the living to teleport to, to follow the action |
| **Repair sabotages** | No |
| **Open or close doors** | No |
| **Report or vote** | No |

**Impostor ghosts** can still **sabotage** (often their only weapon left), but they cannot kill or use vents.

## Settings (`game.yml`)

| Key | Default | Meaning |
|---|---|---|
| `ghost-haunt` | `true` | Give ghosts the Haunt item. |
| `ghost-see-roles` | `false` | Also show the **role** in the Haunt list. Off by default: knowing everything right away takes the interest out of the dead chat. |

Ghosts get the `ghost` [scoreboard](../features/scoreboard.md), which can show how many players are alive and dead. They also keep the [ship map](ship-map.md).

!!! note "No information leaks"
    Ghosts' visual tasks (medbay scan, garbage), the haunt menu and dead players at a repair panel don't give anything away to the living.
