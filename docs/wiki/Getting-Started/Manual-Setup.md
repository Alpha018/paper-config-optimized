# Manual Setup

This guide describes how to apply the optimized configurations to a Minecraft server running **without Docker**.

## Prerequisites

- A running Paper, Spigot, Purpur, or Pufferfish server instance
- Java 21 or later (required for Minecraft 1.21.x)
- The server must have been started at least once so that all configuration files are generated

## Steps

### 1. Identify the Minecraft Version

The server version must be confirmed before proceeding — for example, `1.21.4`. The matching directory in this repository contains the appropriate configuration files.

### 2. Download the Repository

Clone or download this repository to the local machine:

```bash
git clone https://github.com/Alpha018/paper-config-optimized.git
```

### 3. Copy the Configuration Files

All files from the matching version directory must be copied into the server root, replacing the existing ones:

```bash
# Example for version 1.21.4
cp paper-config-optimized/1.21.4/* /path/to/server/
```

The following table lists each file and its expected location within the server directory:

| File | Location |
|---|---|
| `server.properties` | Server root |
| `bukkit.yml` | Server root |
| `spigot.yml` | Server root |
| `paper-global.yml` | `config/paper-global.yml` |
| `paper-world-defaults.yml` | `config/paper-world-defaults.yml` |
| `pufferfish.yml` | Server root |
| `purpur.yml` | Server root |

> **Note:** On Paper-based servers, `paper-global.yml` and `paper-world-defaults.yml` reside inside the `config/` subdirectory, not in the server root.

### 4. Apply JVM Flags

The server startup command must include the appropriate JVM flags for the available RAM allocation. See [[JVM Flags & Startup Commands|JVM-Flags]] for the complete reference.

### 5. Restart the Server

The server must be restarted for the new configuration to take effect.

## Keeping Configurations Updated

When this repository publishes updates for a supported version, steps 2 through 5 should be repeated. Watching the repository for new releases is recommended to stay informed of improvements.

For fully automatic configuration updates, the Docker-based approach is available at [[Quick Start (Docker)|Quick-Start]].
