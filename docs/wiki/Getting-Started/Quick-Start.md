# Quick Start (Docker)

The Docker-based approach is the fastest method to deploy an optimized Minecraft server. It uses the `itzg/docker-minecraft-server` image with the `PAPER_CONFIG_REPO` environment variable to automatically inject all tuned configurations at container startup.

## Prerequisites

- Docker and Docker Compose installed on the host machine
- At least 6 GB of RAM allocated to the container

## Setup

A `docker-compose.yml` file must be created with the following content:

```yaml
services:
  mc:
    image: itzg/minecraft-server
    container_name: minecraft-server
    restart: unless-stopped
    ports:
      - "25565:25565"
    stdin_open: true
    tty: true
    environment:
      EULA: "TRUE"
      TYPE: "PAPER"
      VERSION: "LATEST"
      USE_AIKAR_FLAGS: "true"
      MEMORY: "6G"
      INIT_MEMORY: "6G"
      TZ: "America/Santiago"
      # Automatically injects optimized configs from this repository at startup
      PAPER_CONFIG_REPO: "https://raw.githubusercontent.com/Alpha018/paper-config-optimized/main"
    volumes:
      - ./data:/data
```

Once the file is in place, the server is started with:

```bash
docker compose up -d
```

The container will start, download the Paper server JAR, and apply all optimized configurations automatically.

## How Configuration Injection Works

Upon container startup, `itzg/docker-minecraft-server` fetches the configuration files from `PAPER_CONFIG_REPO`, selecting the directory that matches the running Minecraft version:

```
https://raw.githubusercontent.com/Alpha018/paper-config-optimized/main/<VERSION>/
```

Any improvement merged to the `main` branch of this repository takes effect on the **next container restart** — no manual file management is required.

## Reference Commands

```bash
# Start the server in detached mode
docker compose up -d

# Attach to the interactive server console
docker compose attach mc

# Detach from the console without stopping the server
Ctrl + P, then Ctrl + Q

# Stream server logs
docker compose logs -f mc

# Stop the server gracefully
docker compose stop mc
```

## Recommended Plugins (Auto-Installed)

The provided `docker-compose.yml` pre-installs three plugins via `SPIGET_RESOURCES` on the first boot:

| Plugin | Purpose |
|---|---|
| **FarmControl** | Limits mob density in farms, reducing entity tick cost |
| **Chunky** | Pre-generates chunks to prevent terrain generation lag during player exploration |
| **Spark** | Performance profiler — the primary tool for diagnosing lag causes |

## Next Steps

- [[JVM Flags & Startup Commands|JVM-Flags]] — tuning memory allocation for the host's available RAM
- [[Configuration Overview|Overview]] — understanding what each configuration file controls
- [[Version Compatibility|Compatibility]] — verifying support status for the target Minecraft version
