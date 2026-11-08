# Knife & resource pack

The impostor's kill item can be a small **knife**, and every kill can play a quick **slash**.

## Settings (`game.yml`)

| Key | Default | Meaning |
|---|---|---|
| `knife` | `false` | The kill item becomes a knife. Turn it on **only once players have the pack**: without it they'd see a missing-texture cube. |
| `knife-visible` | `false` | `false` = only the impostor sees the knife (first person and inventory); the others see an **empty hand**, so it gives nothing away. `true` = everyone sees it in the impostor's hand. |
| `kill-animation` | `true` | A quick slash of the kill item in front of the killer and the victim when a kill happens. Only those two see it, nobody is frozen: the body and the cooldown happen at once. Works without the pack too (it slashes with whatever the kill item looks like). |

Without `knife`, the kill item is `items.kill` (`IRON_SWORD`), or `items.kill-disguised` (`CLOCK`) when skins are hidden and `disguise-impostor-items` is on.

## The resource pack

The knife needs the plugin's resource pack. It is written at every start to:

```text
plugins/MRTImposterUs/resourcepack/MRTImposterUs-pack.zip
```

The zip in the folder is replaced only when it is missing or when the jar ships a different pack: if you customise the zip, it is kept until a plugin update brings a new one.

Pick **one** way to get it to your players:

=== "ItemsAdder"

    With ItemsAdder installed (and `resource-pack.itemsadder: true`, the default), the pack's assets are copied by themselves into:

    ```text
    plugins/ItemsAdder/contents/mrtimposterus/resourcepack/
    ```

    1. Run `/iazip` once (and again after plugin updates; the console tells you when files changed).
    2. Make ItemsAdder's pack **required**.
    3. Leave `resource-pack.enabled` at `false`.

    A texture edited directly in ItemsAdder's folder is left alone unless the plugin ships a new pack.

=== "Your own server pack"

    Copy the `assets/` folder of the zip into your server pack and leave `resource-pack.enabled` at `false`.

=== "No pack yet"

    Upload the zip somewhere with a **direct download link** (for example a GitHub release or mc-packs.net), then in `config.yml`:

    ```yaml
    resource-pack:
      enabled: true
      url: 'https://...your direct link.../MRTImposterUs-pack.zip'
      # Empty = the SHA-1 of the zip in the plugin folder
      # (right if you uploaded that zip unchanged).
      sha1: ''
      # true = players who refuse the pack are kicked.
      required: true
    ```

    The plugin sends the pack to every player who joins.

### `resource-pack` keys (`config.yml`)

| Key | Default | Meaning |
|---|---|---|
| `itemsadder` | `true` | Copy the knife into ItemsAdder at every start, when ItemsAdder is installed. |
| `enabled` | `false` | Send the pack to players on join. |
| `url` | `''` | Direct download link of the zip. |
| `sha1` | `''` | SHA-1 of the zip; empty = the one of the zip in the plugin folder. |
| `required` | `false` | Players who refuse the pack are kicked. |

!!! warning "The pack must be required"
    Players who decline the pack see a missing-texture cube in the impostor's hand. When `knife` is on and neither `resource-pack.enabled` + `required` nor ItemsAdder is set up, the console warns you at start. With ItemsAdder it reminds you to make ItemsAdder's pack required.

## Changing the look

Replace the 16×16 texture in the zip:

```text
assets/mrtimposterus/textures/item/knife.png
```

Then upload / rezip the pack again (and update `sha1` if you set it by hand).
