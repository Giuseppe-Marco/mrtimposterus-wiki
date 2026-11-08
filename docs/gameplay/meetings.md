# Meetings & voting

Meetings are where the crew finds the impostors, and where the impostors lie. A meeting gathers everyone at the meeting table, opens the **discussion**, then the **vote**, then the **ejection**.

## Calling a meeting

| How | Rules |
|---|---|
| **Report a body** | Click the body, or use the report item (`PAPER`) to report the nearest body within `report-distance` (default `4.0` blocks). Only living players can report, and not by clicking with the kill item. Who found whom is announced. Reporting a body also cancels a running critical sabotage. |
| **Emergency button** | Click the emergency button. Each player has `emergency-per-player` presses (default `3`) for the **whole match**; they don't recharge. |

The emergency button:

- has a cooldown of `emergency-cooldown-seconds` (default `15`) at the start of the round and after every meeting, so nobody can call two in a row (`0` disables it);
- is **blocked while a sabotage is active** (doors excepted);
- with `button-markers: true` shows a turning red halo while you can press it, and grey smoke while it's blocked or you have no meetings left (drawn per player).

Staff can also open a meeting with `/iu admin meeting [arena]`.

## At the table

- Everyone is teleported to their own **meeting seat** and held there; the camera turns freely.
- Everyone is pushed out of the [vents](vents.md).
- Task markers are hidden.
- Everyone can chat (`meeting-prefix`); ghosts keep their own channel. See [Chat](../features/chat.md).

## Discussion

For `discussion-seconds` (default `30`) nobody can vote yet. To see who is alive and who is dead **during the discussion too**, there are two helpers in `meeting.yml` (turn on one or both):

| Key | Default | What it does |
|---|---|---|
| `player-list-item` | `true` | Everyone gets a **Players** item (`NAME_TAG`, hotbar slot 8) when the meeting opens; it is taken back when the meeting ends. It opens the ballot **read-only** (alive and dead, nothing to click). Close it with ++e++ or ++esc++ to go back to the chat. |
| `tab-player-list` | `true` | The tab list **footer** lists alive (in their colour) and dead (grey, struck through) for the whole meeting. Turn it off if another plugin (e.g. TAB) manages the tab list header and footer. |

## Voting

For `voting-seconds` (default `45`) the living vote on the **ballot**: one head per player (coloured wool instead of heads with [anonymous names](anonymity.md)), plus a **skip** button. Voting closes early when everyone has voted.

- The ballot stays open after voting, so you can watch the other votes come in.
- You can close the ballot to type in chat and **reopen it with the Players item**.
- **No vote by the end counts as a skip.**
- With `announce-vote-cast: true`, everyone is told when a player has voted (not for whom).

| Key | Default | Meaning |
|---|---|---|
| `live-votes` | `true` | **Live votes**: each head shows how many votes it has received and who has already decided, updating with every vote. `false` = **blind voting**: the counts are hidden until the result. |
| `show-votes` | `true` | Show the vote count at the end of the voting. |

Staff can skip ahead with `/iu admin endvote` (opens the vote during the discussion, counts the votes during the vote) or change the timer with `/iu admin settime`.

## Ejection

The most voted player is ejected. **A tie or a skip ejects nobody.**

- With `reveal-role: true` the role of the ejected player is revealed ("was an impostor" / "was not"). Set `false` for a much tenser match.
- A vote for a player who died or left during the vote does not eject anyone.
- The ejected player becomes a [ghost](ghosts.md).
- Ejecting the last impostor wins the match for the crew; ejecting enough innocents to leave the impostors in the majority wins it for the impostors.

After the meeting everyone is sent back onto the map and free play resumes.
