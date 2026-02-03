# 🛠️ Server Optimization & JVM Configuration

Optimizing your Minecraft server involves more than just assigning RAM. The Java Virtual Machine (JVM) flags play a critical role in garbage collection efficiency, reducing lag spikes, and ensuring smooth gameplay.

This guide covers the industry-standard optimizations (often referred to as "Aikar's Flags") and hardware recommendations.

## ☕ Optimized JVM Launch Flags

Using the correct startup flags can significantly improve performance, especially for servers with high player counts.

### 🔹 Essential Flags Explained

| Flag | Function |
| :--- | :--- |
| **-Xms / -Xmx** | Sets the minimum and maximum heap size. **Always set these to the same value** (e.g., `-Xms8G -Xmx8G`) to prevent the JVM from resizing the heap at runtime, which causes lag. |
| **-XX:+UseG1GC** | The G1 Garbage Collector is the most stable and performant option for Minecraft servers running on modern Java versions. |
| **-XX:+ParallelRefProcEnabled** | Optimizes how the GC handles reference processing, using multiple threads to speed up the task. |
| **-XX:MaxGCPauseMillis=200** | Tells the GC to aim for pause times of 200ms or less, preventing noticeable "freeze" lag. |
| **-XX:+AlwaysPreTouch** | Forces the OS to allocate the memory to the process during startup rather than when it's first used. This increases startup time slightly but prevents lag spikes during gameplay. |
| **-XX:+DisableExplicitGC** | Ignores plugins calling `System.gc()`, which is a bad practice and triggers full garbage collection unnecessarily. |

### 🚀 Recommended Startup Script

For a server with **8GB to 12GB of RAM**, use the following configuration. This includes the full suite of optimizations known as "Aikar's Flags".

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

> **Note:** If you are using **Velocity** or another proxy, you might consider `-XX:+UseShenandoahGC` for ultra-low latency, but G1GC is preferred for backend servers.

---

# 🖥️ Hardware Resources Guide

Understanding the balance between CPU and RAM is key to a lag-free experience.

## 🧠 CPU vs. RAM

*   **CPU is King:** Minecraft's main loop is primarily single-threaded. A CPU with high **single-core performance** (high clock speed/IPC) is far more important than a CPU with many cores.
*   **RAM is not a Magic Fix:** Adding too much RAM (e.g., 32GB for a survival server) can actually **hurt performance**. A larger heap means the Garbage Collector has more data to scan, leading to longer lag spikes when it runs.

## 📊 Memory Recommendations

Allocations depend on your server software, plugin count, and player base.

### 🌲 Vanilla / Fabric / Forge
*   **Small Group (1-5 players):** 4GB
*   **Modded (Light):** 6GB
*   **Modded (Heavy):** 8GB - 12GB+ (Depends heavily on mod count)

### 📄 Paper / Pufferfish / Purpur (Optimized)
*   **Survival (SMP):** 6GB - 10GB (Supports 30-50+ players)
*   **Skyblock / Factions:** 8GB - 12GB (Due to high entity counts and complex plugins)
*   **BungeeCord / Velocity:** 512MB - 1GB (Proxies require very little RAM)

## ⚠️ Important Considerations

1.  **Don't Over-allocate:** Stick to 6-10GB for most standard servers. Only go higher if you have specific needs (huge modpacks or 100+ players).
2.  **Pre-generate your world:** Use a plugin like Chunky to pre-generate chunks. This reduces CPU usage significantly when players explore.
3.  **Storage:** Always use an **NVMe SSD**. HDDs are too slow for chunk loading/saving.
