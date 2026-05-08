# paper-world-defaults.yml

Paper's per-world defaults. These values apply to every world unless overridden in a world-specific config.

## Spawning

### per-player-mob-spawns

```yaml
spawning:
  per-player-mob-spawns: true
```

The spawning setting with the greatest effect on total mob count. Instead of a single global mob cap for the entire server, each player receives an individual budget. This means:

- A server with 20 players doesn't maintain 20× the mobs of a solo server
- Players in different biomes each get appropriate mob populations
- Total mob count scales reasonably with online player count

### Despawn ranges

```yaml
despawn-ranges:
  monster:
    soft: 28
    hard: 56
```

Mobs beyond `hard` range despawn immediately. Mobs between `soft` and `hard` have a random chance to despawn each tick. Our values keep mobs close to players and despawn distant ones aggressively.

### Alt item despawn rate

```yaml
alt-item-despawn-rate:
  enabled: false
```

Disabled by default. Enable this to make specific item types (e.g., cobblestone from mining) despawn faster than the standard 5-minute timer. Useful for servers with heavy mining or farming activity.

## Collisions

```yaml
collisions:
  max-entity-collisions: 3
```

Limits how many entities a single entity checks collisions against per tick. Prevents the O(n²) collision cost in cramped mob farms. Set to 3 — enough for normal gameplay, a fraction of the cost in stacked-mob scenarios.

## Hopper

```yaml
hopper:
  cooldown-when-full: true
  disable-move-event: false
```

`cooldown-when-full: true` — when a hopper's output is blocked, it enters a cooldown and skips transfer checks until the cooldown expires. Eliminates a major source of wasted tick time in hopper-based systems.

## Anti-Xray

```yaml
anticheat:
  anti-xray:
    enabled: false
    engine-mode: 1
```

Disabled by default. Engine mode 1 (hide specified blocks) adds negligible overhead. Engine mode 2 (replace all blocks with fake ones) defeats Xray clients reliably but carries a measurable CPU and bandwidth cost — it must be benchmarked before enabling in production.

## Fixes

```yaml
fixes:
  disable-unloaded-chunk-enderpearl-exploit: true
  split-overstacked-loot: true
```

`disable-unloaded-chunk-enderpearl-exploit` prevents players from teleporting through unloaded chunk boundaries, which could cause server instability.

## Tick rates

```yaml
tick-rates:
  mob-spawner: 1
  grass-spread: 1
  container-update: 1
  sensor:
    villager:
      secondarypoisensor: 40
```

The villager `secondarypoisensor` at 40 ticks (2 seconds) instead of every tick cuts villager AI cost without any visible change in behavior.
