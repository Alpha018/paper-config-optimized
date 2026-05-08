# PVP Server Configuration

A PVP-focused server prioritizes combat responsiveness and fairness over the performance trade-offs that the SMP baseline accepts. Several default settings in the baseline actively degrade the PVP experience — they were chosen to reduce CPU cost, not to support competitive combat.

This guide documents only the settings that differ from the [[base configuration|Overview]]. All other values remain at their SMP defaults.

---

## Required overrides

### `paper-global.yml`

#### `collisions.enable-player-collisions`

| | Value |
|---|---|
| **Baseline** | `false` |
| **PVP override** | `true` |

Player-to-player collision is disabled in the SMP baseline to reduce entity processing cost. On a PVP server, collision is a core mechanic — it allows players to knock each other into hazards, block movement in corridors, and use knockback effectively. This must be re-enabled.

---

### `paper-world-defaults.yml`

#### `entities.behavior.disable-player-crits`

| | Value |
|---|---|
| **Baseline** | `false` |
| **PVP override** | `false` (no change required) |

Critical hits are already enabled in the baseline. Verify this value has not been modified in any world-specific config override.

#### `misc.shield-blocking-delay`

| | Value |
|---|---|
| **Baseline** | `5` |
| **PVP override** | `0` |

The baseline introduces a 5-tick (~250ms) delay before a shield activation is registered server-side. This was added to reduce the window for shield-spamming exploits but directly penalizes legitimate defensive play in competitive PVP. Setting it to `0` restores vanilla shield timing.

#### `collisions.allow-player-cramming-damage`

| | Value |
|---|---|
| **Baseline** | `false` |
| **PVP override** | `true` (optional) |

Enables damage when too many entities occupy the same space. Relevant for servers with cramming-based traps or Factions-style mechanics. For pure arena PVP this is optional — enable only if the game mode intentionally uses cramming as a mechanic.

#### `collisions.max-entity-collisions`

| | Value |
|---|---|
| **Baseline** | `3` |
| **PVP override** | `8` |

The baseline caps entity collision checks at 3 to cut CPU cost in mob farms. In PVP, players stacking in tight spaces (chokepoints, siege scenarios) need accurate collision resolution. Raising to `8` covers the vast majority of realistic player stacking scenarios without the full cost of uncapped collisions.

---

### `spigot.yml`

#### `settings.moved-too-quickly-multiplier`

| | Value |
|---|---|
| **Baseline** | `99999999999` |
| **PVP override** | `10.0` |

The baseline sets an extremely permissive threshold to avoid kicking legitimate players on lagged connections. On a competitive PVP server, this effectively disables movement speed anti-cheat. A value of `10.0` is the recommended balance: it tolerates normal lag compensation while flagging genuine speed hacks. Adjust downward (toward `5.0`) on low-latency servers with a strict anti-cheat plugin in place.

#### `settings.moved-wrongly-threshold`

| | Value |
|---|---|
| **Baseline** | `1.0625` |
| **PVP override** | `1.0625` (no change required) |

The baseline already sets a permissive value here (the Spigot default is `0.0625`). No further change is needed for PVP.

#### `world-settings.default.entity-activation-range` (players section)

Players are always fully active regardless of activation range — this setting does not affect player tick rate. No override needed for player detection. However, if the server uses combat-adjacent mobs (golems, wolves, horses), consider the activation ranges documented in [[spigot.yml|spigot-yml]].

---

### `server.properties`

#### `difficulty`

| | Value |
|---|---|
| **Baseline** | `normal` |
| **PVP override** | `hard` (recommended) |

Hard difficulty increases mob damage output and enables additional mob behaviors (zombies breaking doors, etc.). On PVP servers where environmental threat is part of the gameplay, hard difficulty is the standard. Change to match the server's design.

#### `pvp`

| | Value |
|---|---|
| **Baseline** | `true` |
| **PVP override** | `true` (no change required) |

Already enabled in the baseline.

---

## Recommended plugins for PVP servers

These plugins are not part of the configuration files in this repository, but they are commonly required for a functional competitive PVP experience:

| Plugin | Purpose |
|---|---|
| **Grim Anticheat** | Movement and combat anti-cheat designed for 1.21.x — works alongside the `moved-too-quickly-multiplier` adjustment above |
| **Spark** | Already in the baseline — essential for diagnosing combat-related lag spikes |
| **EssentialsX** or equivalent | Spawn, teleport, and combat tag management |

---

## Performance note

The overrides in this guide partially reverse some baseline performance optimizations (player collision, entity collision cap). On a PVP server, the typical player-to-mob ratio is higher and the player-to-player interaction rate is much higher than on an SMP server. Monitor TPS using Spark after applying these overrides and adjust `max-entity-collisions` downward if collision processing becomes a bottleneck during large fights.
