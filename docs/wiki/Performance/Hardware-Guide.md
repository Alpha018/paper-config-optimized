# Hardware Guide

Server hardware selection has a greater impact on Minecraft performance than any individual configuration setting.

## CPU vs. RAM — Priority

**CPU single-core performance is the primary bottleneck.** Minecraft's main game loop is fundamentally single-threaded. High clock speed and strong IPC (Instructions Per Clock) are far more valuable than a high core count.

- A server equipped with a modern 4-core CPU running at 5 GHz will outperform an older 32-core CPU at 2.5 GHz for Minecraft workloads.
- Additional cores benefit asynchronous tasks (chunk I/O, Pufferfish async spawning), but the critical path remains the single main thread.

**RAM allocation is not a substitute for CPU performance.** Allocating more memory than the server requires can degrade performance:

- A larger heap increases the amount of memory the G1 garbage collector must scan during each GC cycle.
- Longer GC scans result in longer freeze spikes visible to players.
- For the majority of survival servers, an allocation between 6 GB and 10 GB provides the optimal balance.

## Memory Recommendations

### Paper / Pufferfish / Purpur

| Use Case | Recommended RAM |
|---|---|
| Small private server (1–10 players) | 4–6 GB |
| Standard SMP (20–40 players) | 6–10 GB |
| Large public server (50+ players) | 10–12 GB |
| Skyblock / Factions (high entity count) | 10–14 GB |

### Proxy (Velocity / BungeeCord)

| Configuration | Recommended RAM |
|---|---|
| Any player count | 512 MB – 1 GB |

Proxy servers handle only connection routing and do not run the game loop. They require minimal memory regardless of player count.

### Vanilla / Fabric / Forge

| Use Case | Recommended RAM |
|---|---|
| Vanilla (1–5 players) | 4 GB |
| Light modpack | 6 GB |
| Heavy modpack (100+ mods) | 10–16 GB |

Heavily modded servers may require considerably more memory depending on the number and complexity of installed mods.

## Storage

An **NVMe SSD** is the recommended storage medium for all Minecraft server deployments. Chunk loading and saving are I/O-intensive operations.

| Storage Type | Suitability |
|---|---|
| HDD | Not recommended — chunk writes can stall the main thread during world generation and heavy player movement |
| SATA SSD | Acceptable for smaller deployments; NVMe is preferred |
| NVMe SSD | Recommended — chunk I/O ceases to be a performance bottleneck |

## World Pre-Generation

The **Chunky** plugin should be used to pre-generate the world before the server is made available to players.

```
/chunky start
```

World generation is CPU and I/O intensive. When players explore ungenerated terrain, the server must generate chunks synchronously on the main thread, producing visible lag. Pre-generation eliminates this problem entirely for the configured radius.

A pre-generated radius of 5,000 to 10,000 blocks is sufficient for the vast majority of player exploration across a typical server lifetime.

## Network

Minecraft traffic is relatively low in bandwidth consumption. A standard 100 Mbps connection is sufficient for 100 or more simultaneous players. Network latency has a greater effect on player experience than throughput — the server should be co-located geographically close to the intended player base.
