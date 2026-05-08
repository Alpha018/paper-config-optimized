# spigot.yml

The Spigot configuration file is where the largest mob-related CPU gains are found. The entity activation and tracking range sections alone can cut entity-related CPU consumption in half on servers with many loaded mobs.

## entity-activation-range

Mobs outside this range from any player become "dormant" — they stop ticking their AI. This is the single biggest mob-related optimization available.

```yaml
entity-activation-range:
  monsters: 24
  animals: 16
  villagers: 16
  flying-monsters: 48
  raiders: 48
  misc: 8
  water: 8
  tick-inactive-villagers: false
```

**Key notes:**
- `tick-inactive-villagers: false` is critical. Villagers are the most expensive mobs to tick — pathfinding, trading cooldowns, gossip. Disabling ticking for out-of-range villagers is a major CPU saving.
- Raiders and flying monsters have higher ranges because their gameplay is long-distance by design.
- `wake-up-inactive` settings let sleeping mobs briefly wake up to prevent them from being completely frozen.

## entity-tracking-range

How far (in blocks) entities are sent to clients. Reducing these reduces bandwidth and client-side processing without affecting server-side simulation.

```yaml
entity-tracking-range:
  players: 48
  animals: 48
  monsters: 48
  misc: 32
  display: 128
  other: 64
```

## nerf-spawner-mobs

```yaml
nerf-spawner-mobs: true
```

Mobs that come from spawners have most of their AI disabled. They exist but cost almost nothing to tick. Essential for mob farms.

## Hopper settings

```yaml
hopper-amount: 1
ticks-per:
  hopper-transfer: 8
  hopper-check: 8
```

Hoppers transfer items every 8 ticks instead of every tick, and only check for items every 8 ticks. On servers with many hopper-based farms this dramatically reduces tick time.

## save-user-cache-on-stop-only

```yaml
save-user-cache-on-stop-only: true
```

The user name cache (`usercache.json`) is written only when the server stops, not on every player join/leave. Eliminates unnecessary I/O on busy servers.

## merge-radius

```yaml
merge-radius:
  item: 3.5
  exp: 4.0
```

Items and XP orbs within this radius are merged into a single entity. Reduces entity count in drop-heavy situations (mob farms, mining).

## mob-spawn-range

```yaml
mob-spawn-range: 3
```

Mobs only spawn within 3 chunks of a player (instead of vanilla's 8). Combined with the reduced `simulation-distance`, this keeps mob populations manageable.
