# JVM Flags & Startup Commands

JVM flags shape server performance as much as the configuration files do. The garbage collector is the direct cause of most "freeze" lag spikes on Minecraft servers — tuning it correctly eliminates the majority of those pauses.

## Aikar's Flags

These flags represent the industry standard for Paper-based servers. The `itzg/docker-minecraft-server` image applies them automatically when `USE_AIKAR_FLAGS: "true"` is set in the environment.

### Key flags explained

| Flag | Purpose |
|---|---|
| `-Xms` / `-Xmx` | Min/max heap size. **Always set them equal** to prevent heap resizing at runtime, which causes lag. |
| `-XX:+UseG1GC` | G1 garbage collector — the best option for Minecraft on modern Java. |
| `-XX:+ParallelRefProcEnabled` | Processes references in parallel during GC, reducing pause duration. |
| `-XX:MaxGCPauseMillis=200` | Target GC pause of 200ms or less. The GC tries to stay under this. |
| `-XX:+AlwaysPreTouch` | Forces the OS to allocate all heap memory at startup. Slightly slower startup, but prevents lag spikes from on-demand memory allocation during gameplay. |
| `-XX:+DisableExplicitGC` | Ignores `System.gc()` calls from plugins. Plugins calling this manually trigger a full GC unnecessarily. |
| `-XX:G1HeapRegionSize` | G1 divides the heap into regions. Larger regions = fewer regions to manage. Use 8M up to 12GB RAM, 16M above that. |

## Startup Commands by RAM

> `-Xms` and `-Xmx` must always be set to the same value.

### 4 GB — Small servers (1–5 players)

```bash
java -Xms4G -Xmx4G \
    -XX:+UseG1GC \
    -XX:+ParallelRefProcEnabled \
    -XX:MaxGCPauseMillis=200 \
    -XX:+UnlockExperimentalVMOptions \
    -XX:+DisableExplicitGC \
    -XX:+AlwaysPreTouch \
    -XX:G1NewSizePercent=30 \
    -XX:G1MaxNewSizePercent=40 \
    -XX:G1HeapRegionSize=8M \
    -XX:G1ReservePercent=20 \
    -XX:G1HeapWastePercent=5 \
    -XX:G1MixedGCCountTarget=4 \
    -XX:InitiatingHeapOccupancyPercent=15 \
    -XX:G1MixedGCLiveThresholdPercent=90 \
    -XX:G1RSetUpdatingPauseTimePercent=5 \
    -XX:SurvivorRatio=32 \
    -XX:+PerfDisableSharedMem \
    -XX:MaxTenuringThreshold=1 \
    -Dusing.aikars.flags=https://mcflags.emc.gs \
    -Daikars.new.flags=true \
    -jar paper.jar nogui
```

### 8–10 GB — Recommended (20–40 players)

```bash
java -Xms8G -Xmx8G \
    -XX:+UseG1GC \
    -XX:+ParallelRefProcEnabled \
    -XX:MaxGCPauseMillis=200 \
    -XX:+UnlockExperimentalVMOptions \
    -XX:+DisableExplicitGC \
    -XX:+AlwaysPreTouch \
    -XX:G1NewSizePercent=30 \
    -XX:G1MaxNewSizePercent=40 \
    -XX:G1HeapRegionSize=8M \
    -XX:G1ReservePercent=20 \
    -XX:G1HeapWastePercent=5 \
    -XX:G1MixedGCCountTarget=4 \
    -XX:InitiatingHeapOccupancyPercent=15 \
    -XX:G1MixedGCLiveThresholdPercent=90 \
    -XX:G1RSetUpdatingPauseTimePercent=5 \
    -XX:SurvivorRatio=32 \
    -XX:+PerfDisableSharedMem \
    -XX:MaxTenuringThreshold=1 \
    -Dusing.aikars.flags=https://mcflags.emc.gs \
    -Daikars.new.flags=true \
    -jar paper.jar nogui
```

### 12–16 GB — Heavy load (50+ players)

*Note: `G1HeapRegionSize` increases to 16M above 12GB.*

```bash
java -Xms12G -Xmx12G \
    -XX:+UseG1GC \
    -XX:+ParallelRefProcEnabled \
    -XX:MaxGCPauseMillis=200 \
    -XX:+UnlockExperimentalVMOptions \
    -XX:+DisableExplicitGC \
    -XX:+AlwaysPreTouch \
    -XX:G1NewSizePercent=30 \
    -XX:G1MaxNewSizePercent=40 \
    -XX:G1HeapRegionSize=16M \
    -XX:G1ReservePercent=20 \
    -XX:G1HeapWastePercent=5 \
    -XX:G1MixedGCCountTarget=4 \
    -XX:InitiatingHeapOccupancyPercent=15 \
    -XX:G1MixedGCLiveThresholdPercent=90 \
    -XX:G1RSetUpdatingPauseTimePercent=5 \
    -XX:SurvivorRatio=32 \
    -XX:+PerfDisableSharedMem \
    -XX:MaxTenuringThreshold=1 \
    -Dusing.aikars.flags=https://mcflags.emc.gs \
    -Daikars.new.flags=true \
    -jar paper.jar nogui
```

### Proxy (Velocity / BungeeCord)

Proxies need very little RAM and benefit from a different GC profile:

```bash
java -Xms1G -Xmx1G \
    -XX:+UseG1GC \
    -XX:G1HeapRegionSize=4M \
    -XX:+UnlockExperimentalVMOptions \
    -XX:+ParallelRefProcEnabled \
    -XX:+AlwaysPreTouch \
    -XX:MaxInlineLevel=15 \
    -jar velocity.jar
```

## Common mistakes

- **Setting `-Xmx` much higher than `-Xms`**: The JVM starts small and grows, causing GC pauses each time the heap expands.
- **Allocating too much RAM**: A 32GB heap means the GC has to scan 32GB. A longer scan = longer pauses. Most servers perform better at 8–12GB than at 32GB.
- **Using `-XX:+UseZGC` or `-XX:+UseShenandoahGC`**: These low-latency collectors work well for proxies but are not recommended for Paper backend servers on current versions.
