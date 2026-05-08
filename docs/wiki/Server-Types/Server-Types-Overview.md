# Server Type Configurations

The base configurations in this repository are optimized for **general survival (SMP)** servers — a balanced setup that prioritizes performance and vanilla-accurate gameplay for 20–50 players.

Different server types have different priorities. A PVP server requires responsive combat mechanics that the SMP baseline intentionally throttles for performance. A Skyblock server deals with extreme entity density that the baseline does not anticipate. Each type requires a targeted set of overrides on top of the base config.

## How to use this section

1. Start with the base configuration for the target Minecraft version (e.g., `1.21.4/`).
2. Identify the server type from the list below.
3. Apply only the overrides documented for that type — do not replace the entire config.
4. Values not listed in the override guide remain at their baseline defaults.

## Available Server Type Guides

| Server Type | Primary concern | Guide |
|---|---|---|
| **PVP** | Combat responsiveness, player collision, anti-cheat tolerance | [[PVP Server|Server-Types-PVP]] |

> Additional server type guides (Skyblock, Factions, Minigames, Creative) will be added as they are tested and contributed by the community. See [[How to Contribute|Contributing]] if a type relevant to your use case is missing.

## What the overrides change — and what they don't

Each override guide specifies:
- **The file** containing the setting
- **The key** to change
- **The new value** and the baseline value it replaces
- **The reason** the change is necessary for that server type

Settings not listed in an override guide are **intentionally left at the SMP baseline**. Changing additional settings without understanding their interaction with the server type is discouraged.
