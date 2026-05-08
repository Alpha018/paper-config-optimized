# purpur.yml

Purpur extends Paper with a large set of gameplay and behavior customization options. Most performance-critical settings in this project are handled by the layers below (Paper, Pufferfish, Spigot). Purpur's value is in the gameplay flexibility it adds on top.

## Settings active in our configs

### Block fall multipliers

```yaml
settings:
  block-fall-multipliers:
    minecraft:hay_block:
      damage: 0.2
    minecraft:*_bed:
      distance: 0.5
```

Hay blocks reduce fall damage to 20% of normal. Beds cut the fall distance calculation in half. Both replicate vanilla behavior introduced across multiple releases and are safe to keep enabled.

### Mob behavior

Our configs leave most Purpur mob settings at their defaults to preserve vanilla gameplay feel. If you want to customize mob behavior (e.g., rideable mobs, custom drops, changed AI), Purpur's configuration is where to do it.

See the [Purpur documentation](https://purpurmc.org/docs/Configuration/) for the full list of available options.

## Performance-relevant Purpur settings to be aware of

While we don't change these from defaults, be careful when adjusting:

- `settings.blocks.*` — some block behavior overrides can cause unexpected interactions with other config layers
- `mobs.<type>.spawn-limits` — Purpur can override per-mob spawn limits; conflicting values with `bukkit.yml` can cause confusion

## Config version

```yaml
config-version: 43
```

Purpur increments this automatically when it migrates config schemas. Do not edit this manually.
