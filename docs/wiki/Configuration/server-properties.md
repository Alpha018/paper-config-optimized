# server.properties

Vanilla Minecraft server settings. These values are set by Mojang and apply regardless of which server software you run.

## Key settings in our configs

| Setting | Value | Reason |
|---|---|---|
| `view-distance` | `6` | Sends chunks up to 6 chunks from the player. Spigot overrides this per-world; see `spigot.yml`. |
| `simulation-distance` | `2` | Mobs, redstone, and crops only tick within 2 chunks of a player. Major CPU reduction. |
| `enable-rcon` | `false` | Disabled for security. Enable only if you have a specific need and use a strong password. |
| `enforce-whitelist` | `true` | Kicks non-whitelisted players on reload. |
| `spawn-protection` | `0` | Disabled — use a permissions plugin to protect spawn instead. |
| `online-mode` | `true` | Always keep this `true` unless behind a proxy (BungeeCord/Velocity) that handles authentication. |
| `network-compression-threshold` | `512` | Packets larger than 512 bytes are compressed. Lower values reduce bandwidth at the cost of CPU. |
| `sync-chunk-writes` | `true` | Ensures chunk data is flushed to disk synchronously. Safer for crash recovery. |
| `max-tick-time` | `60000` | Watchdog kills the server after 60 seconds of a frozen tick. |

## Settings Left at Defaults

- `allow-flight`: `false` — let your anticheat handle this.
- `difficulty`: `normal`
- `pvp`: `true`
- `enable-command-block`: `false` — enable only if your server design requires it.

## Settings That Must Not Be Changed Without Consideration

- `online-mode=false` must not be set unless the server is deployed behind a properly configured proxy (BungeeCord or Velocity). Disabling it allows any client to join under any username without authentication.
- `enable-rcon=true` must not be enabled with an empty or weak `rcon.password`, as it exposes the server to remote command execution.
