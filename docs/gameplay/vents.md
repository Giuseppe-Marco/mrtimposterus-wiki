# Vents

Vents let impostors move around the ship unseen. Vents are grouped into **networks**: from a vent you can only travel to the other vents of the **same** network.

## Who can use them

| Player | Vents |
|---|---|
| Impostor (alive) | Yes |
| Engineer (optional role) | Yes, with `roles.engineer.can-vent: true` (default) |
| Crewmate | No (a crewmate never sees where the vents are) |
| Ghost | No, impostor ghosts included |

With `show-vent-markers: true` (`game.yml`) vents are marked with particles, visible only to the players who can use them.

## How to use a vent

| Action | What happens |
|---|---|
| **Right-click** a vent block | You enter the vent network: you are hidden from everyone and placed inside the vent. |
| **Click** while inside | You hop to the **next vent** of the same network. |
| **Sneak** while inside | You climb out where you are and become visible again. On a full-block vent you stand on top of it. |

Vents only work during free play.

## While inside a vent

- You are **hidden from everyone** (the plugin uses spectator mode to keep you inside the block).
- The **camera turns freely**, but **walking is locked**: you can only travel between vents by clicking them.
- You **can't kill**, and **nobody can kill you**.
- You **can't sabotage**.
- You can't attach the spectator camera to other players.
- The tab list doesn't give you away (see [Anonymity & tab list](anonymity.md)).
- Everyone is **pushed out of the vents** when a meeting starts and when the match ends.

## Setting up vents

In a setup session:

```text
/iu setup vent <id>
```

then click the vent blocks that belong to that network. `/iu setup removevent <id>` removes a network. Make one network per group of linked vents.
