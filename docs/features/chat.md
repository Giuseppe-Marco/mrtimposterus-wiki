# Chat

MRTImposterUs has its own chat, with the Among Us rules: silence in free play, everyone talks in meetings, and the dead have a channel the living never hear. Everything is in the `chat` section of `config.yml`. Set `chat.enabled: false` if another chat plugin must handle everything.

## Scope

| `chat.scope` | What the plugin handles |
|---|---|
| `iu` (default) | Only the ImposterUs chats: the **hub** (players in `chat.hub-worlds`, or the lobby world when empty) and **every match**. The rest of the server's chat is left as it is. Players in ImposterUs (admins watching a match included) are kept out of the rest of the server's chat. |
| `server` | The whole server's chat. |

## In a match

| Phase | Who talks |
|---|---|
| Waiting lobby, end screen | Everyone. |
| Free play | Depends on `active-chat-mode` (below). |
| Meetings | Everyone (`meeting-prefix`, default `<gold>[MEETING] `). |
| Ghosts | Always their own channel (`dead-prefix`, default `<red>[DEAD] `), in every phase. The living never hear it. |

`active-chat-mode` decides whether the living can talk during free play:

| Value | Meaning |
|---|---|
| `'OFF'` (default) | Silence: you only talk in meetings, like the original. |
| `PROXIMITY` | Only players within `proximity-radius` (default `16.0` blocks) hear you. |
| `ALL` | Everyone hears everyone. |

Match chat is logged to the console with the arena name.

## Formats

| Key | Default | Used |
|---|---|---|
| `format` | `'<#ffffff>%player% <#333333>» <gray>%message%'` | Outside matches. `%player%`, `%message%`, `%world%` and PlaceholderAPI placeholders. Legacy `&a`, hex `<#rrggbb>` and MiniMessage. |
| `game-format` | `'<hex><player> <dark_gray>»</dark_gray> <white><message>'` | In a match. `<hex>` is the name colour: don't close it with `</hex>`. |
| `default-name-color` | `'#ffffff'` | Name colour in the match chat (unless `game.colored-names` is true). |
| `smallcaps` | `false` | Small caps in chat, only for the fixed text of the format. Names and messages stay as written. |
| `allow-player-colors` | `false` | Lets players colour their own text (never click/hover tags). |

With [anonymous names](../gameplay/anonymity.md) players appear as their suit colour in the match chat. PlaceholderAPI values in the chat format are escaped, so nicknames can't inject click or hover actions.

## Private messages

| Command | What it does |
|---|---|
| `/iu msg <player> <text>` | Private message (aliases `/iu w`, `/iu tell`). |
| `/iu r <text>` | Reply to the last private message (alias `/iu reply`). |

- `private-messages: true` enables them (no permission needed).
- `private-messages-in-game: false` (default) blocks them **in and into** a match: otherwise two players could make a deal outside the meeting.

## Moderation

```yaml
chat:
  moderation:
    anti-caps:
      enabled: true
      min-length: 8
      max-percent: 50
    anti-flood:
      enabled: true
      cooldown-seconds: 2
      block-repeats: true
    blacklist:
      enabled: false
      block-message: false
      replacement: '***'
      words: []
```

| Filter | What it does |
|---|---|
| Anti-caps | Lowercases shouted messages (at least `min-length` characters, more than `max-percent`% capitals). |
| Anti-flood | Blocks messages that are too fast (`cooldown-seconds`) or repeated (`block-repeats`). `iu.chat.bypass` skips it. |
| Blacklist | Banned words: with `block-message: true` the message is cancelled, otherwise the word is replaced by `replacement`. |
