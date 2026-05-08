# Version Compatibility

This repository supports the **Minecraft 1.21.x** series.

## Support Status

| Version | Status | Notes |
|---|---|---|
| **1.21.1** | Fully Tested | Hand-tuned baseline |
| **1.21.2** | Fully Tested | Hand-tuned |
| **1.21.3** | Fully Tested | Hand-tuned |
| **1.21.4** | Fully Tested | Hand-tuned, reference for 1.21.5+ |
| **1.21.5** | Generated | Based on 1.21.4; `disable-teleportation-suffocation-check` removed |
| **1.21.6** | Generated | Based on 1.21.4; `disable-teleportation-suffocation-check` removed |
| **1.21.7** | Generated | Based on 1.21.4; `disable-teleportation-suffocation-check` removed |
| **1.21.8** | Generated | Based on 1.21.4; `disable-teleportation-suffocation-check` removed |
| **1.21.9** | Generated | Based on 1.21.4; `disable-teleportation-suffocation-check` removed |
| **1.21.10** | Generated | Based on 1.21.4; `disable-teleportation-suffocation-check` removed |
| **1.21.11** | Generated | Based on 1.21.4; `disable-teleportation-suffocation-check` removed |

**Fully Tested** — The configuration was applied to a running server and verified for correctness, performance impact, and the absence of deprecated/removed keys.

**Generated** — The configuration was derived from the 1.21.4 baseline with known incompatible keys removed. It should work correctly but has not been validated against a live server of that exact version.

## Server software compatibility

Each config file in the stack is only loaded by the server software that owns it:

| File | Required software |
|---|---|
| `server.properties` | Any (vanilla, Paper, Spigot, Purpur) |
| `bukkit.yml` | CraftBukkit, Spigot, Paper, Purpur |
| `spigot.yml` | Spigot, Paper, Purpur |
| `paper-global.yml` | Paper, Pufferfish, Purpur |
| `paper-world-defaults.yml` | Paper, Pufferfish, Purpur |
| `pufferfish.yml` | Pufferfish, Purpur |
| `purpur.yml` | Purpur only |

If you run **Paper** without Purpur, `purpur.yml` is silently ignored. If you run **Spigot** without Paper, both paper and pufferfish configs are ignored. The configs are safe to include regardless — unused files cause no harm.

## Older Minecraft versions

Versions prior to 1.21 are not currently supported. The configuration key schemas change between major versions, and backporting requires full retesting.

If you would like to contribute configs for an older version, open an issue or PR following the [[Contributing guidelines|Contributing]].
