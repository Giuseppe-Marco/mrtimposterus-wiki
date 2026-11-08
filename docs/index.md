# MRTImposterUs

**An Among Us-style minigame for Paper servers**: secret roles, tasks with real minigame interfaces, sabotages, vents, bodies to report and meetings to lie in, all inside Minecraft with no client mods. Every arena is set up in game, with a GUI or a step-by-step chat guide.

[Get it on Modrinth](https://modrinth.com/plugin/mrtimposterus){ .md-button .md-button--primary }
[Install it](getting-started/installation.md){ .md-button }

---

## Where to start

<div class="grid cards" markdown>

-   :material-download: **Installation**

    ---

    Requirements, first start and how to update safely.

    [:octicons-arrow-right-24: Installation](getting-started/installation.md)

-   :material-map-marker-radius: **Your first arena**

    ---

    From an empty map world to a playable ship, with the chat guide or the setup GUI.

    [:octicons-arrow-right-24: Your first arena](getting-started/first-arena.md)

-   :material-knife: **How a match works**

    ---

    Waiting lobby, role reveal, free play, meetings and the end of the round.

    [:octicons-arrow-right-24: How a match works](gameplay/match.md)

-   :material-clipboard-check: **Tasks & sabotages**

    ---

    The 14 task minigames, the 5 sabotages and the vent networks.

    [:octicons-arrow-right-24: Tasks](gameplay/tasks.md)

-   :material-account-voice: **Meetings & voting**

    ---

    Reports, the emergency button, discussion, live or blind votes and ejection.

    [:octicons-arrow-right-24: Meetings & voting](gameplay/meetings.md)

-   :material-console: **Commands & permissions**

    ---

    Every command under `/iu`, and the permission each one needs.

    [:octicons-arrow-right-24: Commands](reference/commands.md)

-   :material-shield-account: **Admin tools**

    ---

    Admin panel, live alerts, match log, spectate, pause, forced endings and the debug report.

    [:octicons-arrow-right-24: Admin tools](reference/admin-tools.md)

-   :material-code-braces: **Placeholders**

    ---

    PlaceholderAPI placeholders for TAB, scoreboards and holograms.

    [:octicons-arrow-right-24: Placeholders](reference/placeholders.md)

</div>

## Features at a glance

| | |
|---|---|
| **Roles** | Crewmates and impostors, plus the optional sheriff and engineer |
| **Tasks** | 14 minigames, each with its own interface, with markers visible only to you |
| **Sabotages** | Lights, communications, reactor, oxygen and doors |
| **Vents** | Vent networks for impostors (and engineers), hidden from everyone else |
| **Meetings** | Body reports, a multi-block emergency button, assigned seats, live or blind voting, ejection |
| **Ghosts** | The dead keep doing tasks, have their own chat and can haunt the living |
| **Knife & ship map** | A knife only its holder sees, a kill slash, and a map of the ship in the off hand |
| **Anonymity** | Suit colours, hidden name tags and skins, anonymous names, a tab list that never gives deaths away |
| **Server** | Hub items, arena menu, `[IU]` signs, Citizens NPCs, separate chats, plugin-managed scoreboards |
| **Progression** | Stats, levels, XP, coins and leaderboards (SQLite or MySQL), reward commands |
| **Staff** | Admin panel, match pages, live alerts, match log, streamer mode |

!!! info "Compatibility"
    Paper **1.21.4 – 1.21.11** and **26.1 / 26.2**, Java 21, one jar for both.
    Optional: PlaceholderAPI, ProtocolLib, ItemsAdder, Citizens, TAB, Multiverse-Core.
