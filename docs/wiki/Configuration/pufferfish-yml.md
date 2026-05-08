# pufferfish.yml

Pufferfish is a performance-focused Paper fork. Its optimizations operate at the mob AI level, which is typically the primary CPU consumer on survival servers.

## DAB — Dynamic Activation of Brain

```yaml
dab:
  enabled: true
  start-distance: 10
  max-tick-freq: 20
  activation-dist-mod: 7
  blacklisted-entities: []
```

DAB is the flagship Pufferfish feature. It reduces how often distant mobs run their AI ("brain") based on their distance from the nearest player:

- Mobs within `start-distance` (10 blocks) tick their AI at full speed
- Mobs beyond that tick less frequently, scaling up to `max-tick-freq` (every 20 ticks = once per second) at maximum distance
- `activation-dist-mod` controls the steepness of the scaling curve — lower values make the throttle more aggressive; higher values reduce it. The value 7 produces more aggressive throttling than the default of 8

**Effect:** On a server where 90% of loaded mobs are outside 10 chunks of any player, those mobs consume a fraction of their normal CPU budget while still appearing to behave naturally when a player approaches.

The `blacklisted-entities` list may be used to exclude specific mob types from DAB if they exhibit incorrect behavior under reduced tick rates — for example, certain boss mobs with time-sensitive AI routines.

## Async mob spawning

```yaml
enable-async-mob-spawning: true
```

Moves the mob spawn location calculation off the main server thread. Spawn decisions are still applied on the main thread, but the expensive "where can mobs spawn?" check runs asynchronously.

This is safe to enable — Pufferfish uses careful synchronization to avoid race conditions.

## Inactive goal selector throttle

```yaml
inactive-goal-selector-throttle: true
```

Mobs that are already dormant (outside activation range) skip their pathfinding goal evaluation. Goal selection is one of the more expensive per-mob operations, so skipping it for mobs that aren't doing anything is a pure win.

## Suffocation optimization

```yaml
enable-suffocation-optimization: true
```

Reduces how often the server checks whether a mob is suffocating inside a block. Full checks only happen when the mob is actually at risk of being in a block, not every tick for every mob.

## Projectile limits

```yaml
projectile:
  max-loads-per-projectile: 8
  max-loads-per-tick: 10
```

Limits how many chunks a projectile (arrow, fireball, etc.) can force-load per tick and per projectile lifetime. Prevents projectile-based chunk loading exploits.
