# Configuration Overview

Each version directory contains 7 configuration files that form a layered stack from vanilla Minecraft up through the server software. Understanding which layer controls which behavior determines where to look when tuning a specific bottleneck.

## The Config Stack

```
server.properties          ← Vanilla Minecraft (lowest layer)
    └── bukkit.yml         ← CraftBukkit: spawn limits, tick rates
        └── spigot.yml     ← Spigot: entity activation, tracking ranges, world tweaks
            └── paper-global.yml         ← Paper: global networking, chunk loading, packet limits
            └── paper-world-defaults.yml ← Paper: per-world entity behavior, hopper, collisions
                └── pufferfish.yml       ← Pufferfish: async spawning, DAB, AI throttling
                └── purpur.yml           ← Purpur: extended mob/block/player behavior
```

Higher layers override or extend the layers below them. When a setting exists in both `spigot.yml` and `paper-world-defaults.yml`, Paper's value takes precedence.

## File Responsibilities

### `server.properties`
Vanilla settings: port, gamemode, difficulty, view distance, online mode, RCON. Our configs set conservative view/simulation distances here (`view-distance=6`, `simulation-distance=2`) and disable RCON by default.

See [[server.properties reference|server-properties]].

### `bukkit.yml`
Controls global spawn limits per category (monsters, animals, water creatures) and how often the server attempts spawns (`ticks-per`). Our configs drastically reduce ambient/animal/water spawn frequency since these mobs don't need to be checked every tick.

See [[bukkit.yml reference|bukkit-yml]].

### `spigot.yml`
The biggest performance lever after `bukkit.yml`. Controls:
- **Entity activation ranges** — how far a mob must be from a player before it goes "dormant"
- **Entity tracking ranges** — how far entities are sent to clients
- **Hopper transfer rates** — throttled to reduce tick cost
- **Nerfed spawner mobs** — mobs from spawners skip pathfinding entirely, cutting entity tick cost in mob farms

See [[spigot.yml reference|spigot-yml]].

### `paper-global.yml`
Server-wide Paper settings:
- **Chunk loading rate caps** — prevents players from overwhelming the chunk system
- **Packet limiter** — kicks or drops clients sending packets too fast
- **Player collision** — disabled globally to reduce entity processing
- **Misc** — max joins per tick, region file cache size

See [[paper-global.yml reference|paper-global-yml]].

### `paper-world-defaults.yml`
Per-world Paper settings applied as defaults to all worlds:
- **Per-player mob spawns** — fairer mob distribution, reduces total mob count
- **Max entity collisions** — capped at 3 to avoid expensive collision checks in cramped farms
- **Hopper cooldown** — enabled to skip checks when hoppers are blocked
- **Anti-Xray** — disabled by default (use engine-mode 2 if you need it, at a CPU cost)
- **Despawn ranges** — conservative values that keep the world feeling alive without keeping distant mobs ticking

See [[paper-world-defaults.yml reference|paper-world-defaults-yml]].

### `pufferfish.yml`
The file with the greatest CPU reduction effect on crowded servers:
- **DAB (Dynamic Activation of Brain)** — mobs far from players tick their AI less frequently, scaling with distance
- **Async mob spawning** — moves spawn calculations off the main thread
- **Inactive goal selector throttle** — dormant mobs skip expensive pathfinding goal evaluation

See [[pufferfish.yml reference|pufferfish-yml]].

### `purpur.yml`
Purpur-specific extensions to mob, block, and player behavior. Most performance-relevant settings in our config are left at defaults here; Purpur's value comes from gameplay customization options it exposes.

See [[purpur.yml reference|purpur-yml]].
