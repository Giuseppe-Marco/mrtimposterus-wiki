# Sabotages

Impostors can sabotage the ship with their **sabotage item** (`REDSTONE_TORCH`, or `PAPER` when disguised; see `items` in `game.yml`). There are five sabotages, all configured in `sabotage.yml`.

| Sabotage | Type | Effect | Repair |
|---|---|---|---|
| **Lights** | Nuisance | Crewmates are left in the dark (blindness); impostors can see (`impostor-night-vision`). | One panel |
| **Communications** | Nuisance | Hides the task list and the task progress. | One panel |
| **Reactor** | **Critical** | Countdown: if it runs out, the impostors win. | **Two** panels at once |
| **Oxygen** | **Critical** | Countdown: if it runs out, the impostors win. | **Two** panels at once |
| **Doors** | Doors | Seals the doors picked at setup for a few seconds. | Not repairable: they reopen by themselves |

## Rules

- Only **one** sabotage at a time.
- An impostor **cannot** sabotage from inside a vent.
- While a sabotage is active the **emergency button is blocked** (doors are the exception).
- **Reporting a body cancels a critical sabotage**: it is the only way out besides repairing it. Lights and communications stay active even after the meeting.
- **Impostors can repair too** (useful to avoid suspicion).
- **Ghosts cannot repair**, but an **impostor ghost can still sabotage**.

## Timers

| Key | Default | Meaning |
|---|---|---|
| `initial-cooldown-seconds` | `10` | Wait before the first sabotage is available. |
| `cooldown-seconds` | `30` | Wait before the next one. It starts from the **repair**, not from the activation; doors are the exception and restart immediately. |
| `reactor.seconds` | `120` | Time before the meltdown. |
| `oxygen.seconds` | `120` | Time before oxygen runs out. |
| `doors.seconds` | `10` | How long the doors stay closed. |

Every sabotage has its own `enabled` key (default `true`).

## Repair panels

Panels are placed in setup with `/iu setup panel <lights|communications|reactor|oxygen>` while looking at the block (`/iu setup clearpanels <type>` removes them).

- **Critical** sabotages (reactor, oxygen) need **two panels held down at the same time by two different players**.
- A player "holds" the panel they clicked as long as they stay within `hold-radius` (default `3.0` blocks). If they move away, die or leave, the panel is released.
- With `glow-panels: true`, during a critical sabotage the task markers turn off and the repair panels glow with a red outline visible **through walls** from anywhere on the map.
- A sabotage can only be started if the arena has the panels it needs (for the doors: at least one door).

### Lights

| Key | Default | Meaning |
|---|---|---|
| `lights.blindness-amplifier` | `0` | `0` = pitch black; raise it for a lighter darkness. |
| `lights.minigame` | `true` | The panel opens an electrical box with tripped switches that must all be flipped back up. `false` = one click on the panel is enough. |
| `lights.switches` | `5` | Switches in the box (3–5). Some trip at random, at least one is always down. |

### Communications

| Key | Default | Meaning |
|---|---|---|
| `communications.minigame` | `true` | The panel opens a radio: turn the knob until the signal locks onto the right frequency (the bars show how close you are). `false` = one click is enough. |

While communications are down, the task bar, the scoreboard task placeholders and the task stations on the [ship map](ship-map.md) are hidden.

## Doors

1. Build the doors **already closed** in the map (doors, trapdoors or gates).
2. Select them with `/iu setup gui` → Sabotages → "Doors to lock", or with `/iu setup doors` and click them (click again to remove). `/iu setup cleardoors` removes them all.
3. At the start of every match the plugin **opens** them all. The doors sabotage closes them again for `doors.seconds`, then they reopen by themselves.

Ghosts can't open or close doors.

## Admin control

Staff can start a sabotage (ignoring the cooldown, but still needing the arena's panels) or repair the running one with `/iu admin sabotage <lights|communications|reactor|oxygen|doors|fix> [arena]`, and change a critical sabotage's timer with `/iu admin settime`. See [Admin tools](../reference/admin-tools.md).
