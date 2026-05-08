# How to Contribute

Contributions are welcome across all areas of the project — configuration improvements, new version support, and documentation corrections.

## Types of Contributions

### Configuration Improvements

Pull requests proposing changes to existing settings must include:

- The specific file and key being modified
- The previous value compared to the proposed value
- A justification for how the change improves performance or correctness, with a reference to official Paper, Spigot, or Purpur documentation where applicable
- Evidence of testing — a description of the environment and observed behavior is sufficient

### New Version Support

When a new Minecraft 1.21.x version is released, the process for adding support is as follows:

1. Copy the directory of the closest existing version
2. Start a server with the new version and allow it to generate fresh configuration files
3. Compare the generated files against the baseline — document any keys that were added, removed, or renamed
4. Update the copied directory to reflect the new schema
5. Bump `_version` fields in Paper YAML files if the schema version changed
6. Open a pull request specifying whether the configuration is **Fully Tested** (validated against a live server) or **Generated from baseline**

### Documentation

The wiki source files are located in `docs/wiki/`. All pages are authored in Markdown. Changes submitted via pull request are automatically synchronized to the GitHub Wiki upon merge to `main` through the configured GitHub Actions workflow.

## Contribution Process

1. **Open an issue first** for any non-trivial change, to confirm alignment with the project's direction before implementation begins.
2. **Fork** the repository to an independent GitHub account.
3. **Create a branch** with a descriptive name that reflects the scope of the change (e.g., `fix/spigot-entity-range-1.21.5`, `feat/1.21.12`).
4. **Test the changes** against a live server where possible.
5. **Submit a pull request** with a clear description following the Conventional Commits format.

## Commit Convention

This project follows the **Conventional Commits** specification:

```
feat(1.21.5): update spigot entity activation ranges
fix(paper-world-defaults): correct hopper cooldown value
docs: update JVM flags guide for 1.21.x
chore: add 1.21.12 directory from 1.21.4 baseline
```

## Rejection Criteria

The following types of changes will not be merged:

- Changes that break vanilla gameplay mechanics without strong technical justification
- Settings that sacrifice gameplay correctness for marginal performance gains
- Configuration keys that are undocumented or experimental without clear sourcing from official documentation
- Configurations for new Minecraft versions submitted without at minimum a documented test status in the pull request

## Questions

Issues may be opened on GitHub for questions or clarification. The project maintainer reviews issues regularly.
