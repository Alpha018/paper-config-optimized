# Minecraft Server Optimized Configurations

[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://buymeacoffee.com/alpha018)

This repository provides optimized configurations for Spigot and Paper Minecraft servers. The configurations are aimed at reducing lag, improving server performance, and ensuring a secure default setup.

These optimizations are based on:

- [Optimize Paper.yml to Reduce Lag](https://shockbyte.com/billing/knowledgebase/155/Optimize-paperyml-to-Reduce-Lag.html)

## Purpose

This repository is intended to be used with the [Docker Minecraft Server Image](https://github.com/itzg/docker-minecraft-server), offering lag-reducing configurations specifically tailored for **Minecraft 1.21.x** servers.

## Features

- **Optimized Paper, Spigot, and Purpur files** to minimize lag and maximize performance.
- **Security Best Practices**: RCON is disabled by default, and `server.properties` is cleaned of insecure passwords.
- **Consistent & Clean**: All versions are checked for consistency. No spam links or 3rd party ads in configs.
- **Docker compatibility** for easy deployment with the `itzg/docker-minecraft-server` image.
- **Community-driven improvements**: Contributions are welcome! See [Contributing Guidelines](CONTRIBUTING.md) to submit your improvements and configurations.

## Startup Command & JVM Flags

To ensure optimal performance, using the correct Java startup flags is crucial.

For a detailed guide on **JVM Flags**, **RAM allocation**, and **Hardware recommendations**, please read our dedicated guide:

👉 **[JVM Optimization & Flags Guide](JVM_FLAGS.md)**

## Version Compatibility

This repository currently supports the **Minecraft 1.21.x** series, including:

- **1.21.1** to **1.21.4** (Fully Tested & Optimized)
- **1.21.5** to **1.21.11** (Generated based on stable 1.21.4 configs, `disable-teleportation-suffocation-check` removed for compatibility)

If you would like to contribute configurations for older Minecraft versions, they must be thoroughly tested. Please submit a Pull Request (PR) with detailed notes. All contributions should follow the guidelines outlined in the [CONTRIBUTING.md](CONTRIBUTING.md) file.

## How to Use

1. Clone this repository.
2. Select the directory matching your Minecraft version (e.g., `1.21.11`).
3. Replace the configuration files in your Minecraft server's directory with the ones provided.
4. Restart your server for the changes to take effect.

## Contributing

We welcome contributions from the community! If you'd like to add new configurations or optimize existing ones:

1. Thoroughly test your changes.
2. Open a PR with detailed descriptions of your modifications.
3. Ensure your contributions follow the [Contributing Guidelines](CONTRIBUTING.md).

Let's work together to create the best-optimized Minecraft server experience!

## ❤️ Support the Project

If these configurations helped your server run smoother, please consider giving this repository a **⭐ Star**!

It helps other players and server owners find these optimizations and improve their gameplay experience. Every star counts!

If you really love the project and want to support its maintenance, you can also buy me a coffee:

[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://buymeacoffee.com/alpha018)

## Stay in touch

- Author - [Tomás Alegre](https://github.com/Alpha018)
