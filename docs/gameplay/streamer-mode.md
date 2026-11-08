# Streamer mode

Streamers show their screen live, so being drawn as impostor spoils the round for everyone watching the stream. **Streamer mode** is an on/off immunity: while it is on, the player is **never drawn as impostor**.

## Using it

```text
/iu streamer
```

Run it once to turn the immunity on, again to turn it off.

- Permission: **`iu.streamer`**. **Nobody has it by default** (not even through `iu.player`): give it to your streamers. `iu.*` includes it.
- It takes effect from the **next role draw**: the roles of a round already running are not changed (the player is told so).
- `/iu help` shows the command only to players who have the permission.

## Where it is saved

The immunity is saved in the statistics database, table **`iu_streamers`**, so it survives restarts. A statistics reset (`/iu resetstats`) doesn't touch it.

!!! note "Statistics disabled"
    With `stats.enabled: false` there is no database: the immunity lasts only until the next restart.

## Edge cases

| Situation | What happens |
|---|---|
| Too few players can be impostor | The round has **fewer impostors**. |
| Everyone in the round is immune | One of them is drawn anyway. |
| An admin picks the player with `/iu impostor` | The admin's pick **wins** over the immunity. |

`/iu debug` also logs the streamer immunity decisions.
