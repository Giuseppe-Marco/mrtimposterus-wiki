# Tasks

Crewmates win by finishing every task. Each task is a **minigame** with its own interface, done at one or more **stations** (blocks) of the map.

## Task types

| Type | Meaning |
|---|---|
| `COMMON` | Identical for **everyone**: a lie about one of these can be exposed. |
| `LONG` | Several steps or several stations to visit. |
| `SHORT` | Quick, a single interaction. |

How many of each type a player receives is set in `tasks.yml`:

```yaml
tasks:
  common: 1
  long: 2
  short: 3
```

Impostors receive a list of **fake tasks**, so they can pretend. Ghosts keep doing their tasks, and those still count for the crew.

## Creating a task

In a setup session:

```text
/iu setup task <id> [COMMON|SHORT|LONG] [minigame] [room]
```

then click the blocks of its stations. Without a type the task is `SHORT`, without a minigame it is `wiring`. The optional `room` is a name used as a label on the [ship map](ship-map.md). `/iu setup removetask <id>` removes it. A mistyped minigame name is refused.

## The 14 minigames

| Id | Task | How it plays |
|---|---|---|
| `wiring` | Fix wiring | Coloured wires on the left must be connected to their shuffled counterparts on the right: click a wire, then its matching colour. |
| `swipe-card` | Swipe card | Drag the card left to right across the reader, one slot at a time; clicking out of order restarts the swipe. |
| `download` | Download data | A progress bar fills on its own while you stand still with the GUI open; closing it early loses the transfer. |
| `align-engine` | Align engine output | Nudge a marker onto the centre line with the up/down arrows. |
| `fuel` | Fuel engines | Work the lever until the tank is full: each click pumps one measure. |
| `asteroids` | Clear asteroids | Shoot the targets drifting across the viewport. |
| `keypad` | Enter the code | Type the code shown on the keypad in order; a wrong digit clears the entry. |
| `medbay-scan` | Medbay scan | **Visual task.** No GUI: stand still inside a ring of light while everyone nearby sees the animation. Moving away cancels the scan. |
| `garbage` | Empty garbage | **Visual task.** Keep clicking to hold the lever down: rubbish pours out in full view; letting go drains the progress. |
| `canister` | Fill canisters | Take an empty canister, hold it under the tap until full, put it back. |
| `filter` | Clean O2 filter | Pull the leaves out one by one and drop them in the chute; clicking anything else jams the filter for a moment. |
| `simon` | Start reactor sequence | The panel flashes a sequence, then goes dark: repeat it from memory. |
| `calibrate` | Calibrate distributor | Stop the sweeping cursor inside the green window; the window shrinks as you go. |
| `breaker` | Reset breakers | Every breaker must end up ON, but flipping one also flips its neighbours. |

!!! tip "Visual tasks prove innocence"
    Other players see you while you do the **medbay scan** and **empty garbage**, and an impostor cannot fake them. Put them in clearly visible spots. An impostor's medbay scan is visible only to that impostor.

## Minigame tuning

All in `tasks.yml`, under `minigames` (and overridable [per arena](../getting-started/arena-settings.md)):

| Key | Default | Meaning |
|---|---|---|
| `wiring.wires` | `4` | Number of wires (2–4). |
| `download.seconds` | `8` | Seconds of transfer. |
| `fuel.clicks` | `10` | Pumps needed. |
| `asteroids.targets` | `8` | Targets to hit. |
| `keypad.length` | `4` | Digits of the code (2–6). |
| `medbay-scan.seconds` / `max-distance` | `10` / `1.6` | Seconds standing still; moving beyond `max-distance` restarts the scan. |
| `garbage.seconds` | `6` | Seconds of lever held down. |
| `canister.canisters` / `fill-clicks` | `2` / `5` | Canisters, and clicks to fill one. |
| `filter.leaves` | `6` | Leaves to remove (3–10). |
| `simon.length` | `4` | Length of the sequence (3–8). |
| `calibrate.rounds` / `speed-ticks` | `3` / `2` | Hits in the green zone, and how often the cursor moves (lower = faster). |
| `breaker.breakers` / `shuffles` | `5` / `3` | Breakers (3–7) and how shuffled the puzzle is. |

## Task bar and markers

| Key (`tasks.yml`) | Default | Meaning |
|---|---|---|
| `show-bossbar` | `true` | The global completion bar (boss bar). |
| `show-markers` | `true` | The next station of **your** tasks gets a beam of light and a rotating ring, visible only to you. Getting closer shows the task name in the action bar. |
| `marker-radius` | `24.0` | Distance within which the marker is visible. |
| `glow-markers` | `true` | A glowing outline of the station block, visible through walls, in your colour and only to you. Useful when a station is inside a machine or under a low ceiling. |
| `markers-for-impostors` | `false` | Impostors get the fake task list but **no** markers: they must know the map. Set `true` to help newcomers. |

- During the **communications** sabotage the task bar and the task progress are hidden (`?`).
- During a **critical** sabotage the task markers are turned off and the repair panels glow instead (`sabotage.glow-panels`).
- Task markers are hidden during meetings.
