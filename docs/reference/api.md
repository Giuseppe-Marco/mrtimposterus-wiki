# Developer API

Other plugins can read the state of the matches and listen to match events.

Class: **`it.mrt.imposterus.api.ImposterUsAPI`** (static methods).

It exposes the matches with standard types only (`UUID`, `String`, `boolean`, `Set<UUID>`): callers never depend on the plugin's internal classes, which can change between versions. Every method can be called at any time: when the plugin is not enabled or the player is not in a match it returns `false`, `null` or an empty set. Call it from the server **main thread**.

In your plugin's `plugin.yml` add `MRTImposterUs` to `depend` or `softdepend`:

```yaml
softdepend:
  - MRTImposterUs
```

## Methods

| Method | Returns |
|---|---|
| `isInGame(UUID)` | in a game (waiting lobby, countdown or running match) |
| `isInMatch(UUID)` | their match is running (reveal, free play, meeting or voting) |
| `getArenaName(UUID)` | arena name, `null` when not in a game |
| `getGameState(UUID)` | `WAITING`, `COUNTDOWN`, `STARTING`, `ACTIVE`, `MEETING`, `VOTING`, `ENDING`, `RESETTING` or `null` |
| `isInMeeting(UUID)` | a meeting (discussion or voting) is on screen in their game |
| `getPlayers(UUID)` | every player of the same game, the player included |
| `getArenaPlayers(String)` | every player of the game running in that arena |
| `isAlive(UUID)` | alive in the match (not killed, not ejected, not a ghost) |
| `isGhost(UUID)` | dead and roaming the map as a ghost |
| `isImpostor(UUID)` | an impostor (always `false` before the roles are assigned) |
| `getRole(UUID)` | `CREWMATE`, `IMPOSTOR`, `SHERIFF`, `ENGINEER`, `MEDIC`, `SCIENTIST` or `null` |
| `getColor(UUID)` | suit colour name (e.g. `RED`), `null` when none is chosen |
| `areInSameGame(UUID, UUID)` | both players are in the same game |

## Example

```java
import it.mrt.imposterus.api.ImposterUsAPI;

if (ImposterUsAPI.isInMatch(bot.getUniqueId())
        && ImposterUsAPI.isAlive(bot.getUniqueId())
        && ImposterUsAPI.isImpostor(bot.getUniqueId())
        && !ImposterUsAPI.isInMeeting(bot.getUniqueId())) {
    // the bot may look for a victim
}
```

## Events

Package `it.mrt.imposterus.api.event`:

| Event | Fired when | Getters |
|---|---|---|
| `GameStartEvent` | A match goes live: the map is loaded, roles are assigned and the reveal is on screen. | `getGame()` |
| `GameEndEvent` | A match ends, before the end-screen delay and the world teardown. | `getGame()`, `getReason()`, `isDecisive()` (`false` when aborted and nobody won), `isImpostorWin()` |
| `PlayerKilledEvent` | After an impostor kills a crewmate (the victim is already a ghost and the body is dropped). | `getGame()`, `getKiller()`, `getVictim()` |
| `PlayerEjectedEvent` | A meeting votes someone out (not fired on a skip or a tie). | `getGame()`, `getEjected()`, `getVotes()` |

!!! note
    The event getters return the plugin's own match and player objects, which are internal classes and may change between versions. For stable reads, use the `ImposterUsAPI` methods above with the player's UUID.
