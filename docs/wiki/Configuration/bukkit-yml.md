# bukkit.yml

CraftBukkit configuration. The two sections with the greatest effect on server performance are `spawn-limits` and `ticks-per`.

## spawn-limits

Controls the maximum number of each mob category that can exist per world.

```yaml
spawn-limits:
  monsters: 20
  animals: 5
  water-animals: 2
  water-ambient: 2
  axolotls: 3
  ambient: 1
```

These values are set well below vanilla defaults (vanilla uses 70 monsters, 10 animals). The reduction holds up in practice because:

- Paper's `per-player-mob-spawns` distributes this budget fairly among online players
- Most players never notice the difference because mobs still feel plentiful at these levels
- The CPU saved from not processing excess mobs is substantial on busy servers

## ticks-per

Controls how many game ticks pass between each spawn attempt for each category.

```yaml
ticks-per:
  monster-spawns: 20       # Every 1 second
  animal-spawns: 500       # Every 25 seconds
  water-spawns: 500
  water-ambient-spawns: 500
  axolotl-spawns: 500
  ambient-spawns: 500
  autosave: 6000           # Every 5 minutes
```

Monsters still check every second (needed for gameplay feel), but ambient, water, and animal spawns are checked rarely. These mobs are long-lived and don't need to be evaluated frequently.

## chunk-gc

```yaml
chunk-gc:
  period-in-ticks: 300
```

Controls how long chunks force-loaded by plugins remain loaded before being released. This setting has no effect on normal chunk unloading. Paper internally caps the value to 20 ticks (1 second) regardless of what is set here, so any value above 20 behaves identically.
