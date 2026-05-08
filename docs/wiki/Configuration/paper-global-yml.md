# paper-global.yml

Paper's global configuration applies server-wide, regardless of which world a player is in.

## Chunk loading

```yaml
chunk-loading-basic:
  player-max-chunk-load-rate: 100
  player-max-chunk-send-rate: 75
  player-max-chunk-generate-rate: -1

chunk-loading-advanced:
  auto-config-send-distance: true
  player-max-concurrent-chunk-loads: 0
  player-max-concurrent-chunk-generates: 0
```

These caps prevent a single player from flooding the chunk system (for example, by flying at high speed). `player-max-chunk-send-rate: 75` limits how many chunks per second are sent to each client, protecting both server and client performance.

## Packet limiter

```yaml
packet-limiter:
  all-packets:
    action: KICK
    interval: 7
    max-packet-rate: 500
  overrides:
    ServerboundPlaceRecipePacket:
      action: DROP
      interval: 4
      max-packet-rate: 5
```

Kicks clients that send more than 500 packets per 7 seconds. The recipe packet override specifically throttles clients that spam recipe clicks (a common exploit vector).

## Collisions

```yaml
collisions:
  enable-player-collisions: false
```

Player-to-player collision is disabled globally. This removes the entity collision processing cost for all players and is invisible in most gameplay contexts.

## Misc

```yaml
misc:
  max-joins-per-tick: 3
  region-file-cache-size: 256
```

- `max-joins-per-tick: 3` — prevents login storms from stalling the server when many players connect simultaneously.
- `region-file-cache-size: 256` — keeps 256 region files open in memory, reducing disk I/O for chunk reads.

## Spam limiter

```yaml
spam-limiter:
  incoming-packet-threshold: 300
  tab-spam-limit: 500
```

Secondary protection against packet flooding, specifically for tab-completion spam which can be used to cause lag.
