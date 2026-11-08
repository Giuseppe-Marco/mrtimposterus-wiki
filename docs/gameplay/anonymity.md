# Anonymity & tab list

In Among Us you recognise people only by their suit. MRTImposterUs does the same: once the round starts, names, skins and the tab list are handled so that nobody can tell who is under which suit, or who has just died.

## Suit colours

Players pick their colour with the wool in the waiting lobby. The coloured suit (leather armour) is put on only when the round starts: in the waiting lobby everyone keeps their own skin and name.

## Settings (`game.yml`)

| Key | Default | Meaning |
|---|---|---|
| `hide-nametags` | `true` | Hides the name above the head once the match has started (applies to everyone, accomplices included). In the lobby the names stay visible. |
| `hide-skins` | `true` | Players become invisible, but the coloured leather armour stays visible: only the coloured silhouette is seen. |
| `anonymous-names` | `true` | Players go by their **suit colour** instead of their name **everywhere**: chat ("Red » ..."), vote menu (coloured wool instead of the head), reports, ejections, kill and disconnect messages. `false` = real names. |
| `colored-names` | `false` | Only with `anonymous-names: false`: paints player names in the suit colour in the match chat and the vote menu. Off, the name stays neutral (`chat.default-name-color`). |
| `show-corpse-names` | `false` | Shows the victim's name above the body. Off: the body is recognised by its colour. |
| `disguise-impostor-items` | `true` | With hidden skins the held item stays visible: impostor items take the look of harmless objects (`kill-disguised`, `sabotage-disguised`). |

!!! tip "Why `colored-names` is off"
    With real names on, reading "Marco" in red in the chat would tell everyone which suit Marco is wearing, making hidden names and skins pointless.

## Impostors recognising each other

With names and skins hidden, accomplices need a way to find each other:

| Key | Default | Meaning |
|---|---|---|
| `impostor-markers` | `true` | A crown of red particles above the accomplices' heads, visible **only** to the other impostors. No external plugin needed. |
| `impostor-marker-range` | `32.0` | Distance within which the crown is visible. |
| `impostor-glow` | `false` | Impostors glow for the other impostors, **through walls**. Requires **ProtocolLib**; without it, it stays off (a normal glow would be visible to everyone). |

## Tab list

On Paper, hiding a player (a ghost, an impostor in a vent) also removes them from the tab list, which would tell everyone who died. The plugin handles this:

| | Behaviour during a running round |
|---|---|
| **With ProtocolLib** | Everyone in the round **always stays listed**, dead or venting, so the tab list gives nothing away. |
| **Without ProtocolLib** | The players of a running round are **taken off each other's tab list**, so no name can vanish. |

With `tab.per-game-visibility: true` (`config.yml`) each player only sees the players of their own match; players not in a match share the lobby group.

During meetings the tab footer can list alive and dead players (`tab-player-list` in `meeting.yml`, see [Meetings](meetings.md#discussion)).

## Scoreboard

The sidebar is personal: your role is shown only to you. The default `match` board doesn't show how many players are alive, because that would reveal a kill. See [Scoreboard](../features/scoreboard.md).

!!! warning "Using the TAB plugin"
    Suit colours and hidden name tags use scoreboard teams. TAB's `scoreboard-teams` feature blocks them unless you add a `disable-condition`: see [Placeholders](../reference/placeholders.md#names-and-colours-with-the-tab-plugin).
