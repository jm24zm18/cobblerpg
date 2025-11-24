# CLAUDE.md - CobblePRG Modpack Development Guide

## Project Overview

**CobblePRG** is a Minecraft modpack build system that combines a custom Fabric mod with Cobblemon and other third-party mods. The primary goal is to create a repeatable, automated build process that produces a polished, installable modpack compatible with Prism Launcher and potentially other platforms.

### Key Objectives
- Produce a single-click installable modpack ZIP
- Support custom Fabric mods alongside third-party dependencies
- Include custom datapacks and configuration files
- Maintain version pinning for reproducible builds
- Enable both client and server pack generation

---

## Repository Structure

The repository follows a modular organization pattern:

```
cobblerpg/
├── mod/                    # Custom Fabric mod source code
│   ├── src/
│   ├── build.gradle
│   └── fabric.mod.json
├── datapacks/             # Custom Minecraft datapacks
│   └── [datapack_name]/
│       ├── pack.mcmeta
│       └── data/
├── external-mods/         # Third-party mod JARs
│   ├── cobblemon-*.jar
│   ├── fabric-api-*.jar
│   └── [other-mods].jar
├── config/                # Mod configuration files
│   └── [mod-configs].json
├── pack-meta/             # Modpack metadata and versioning
│   ├── VERSIONS.md
│   └── manifest.json (optional)
├── build/                 # Build output directory (gitignored)
│   └── modpack-*.zip
├── build.gradle          # Root Gradle build script
├── settings.gradle       # Gradle project settings
├── PLAN.md              # Development roadmap
├── CLAUDE.md            # This file
└── README.md            # User-facing documentation
```

### Current State
⚠️ **Early Development**: The repository currently contains only planning documentation. The implementation phases are outlined in `PLAN.md`.

---

## Technology Stack

### Core Technologies
- **Minecraft Version**: TBD (likely 1.20.x or 1.21.x)
- **Mod Loader**: Fabric
- **Build System**: Gradle with Fabric Loom
- **Primary Language**: Java (17 or 21)
- **Version Control**: Git

### Key Dependencies
- **Cobblemon**: Pokémon mod for Minecraft
- **Fabric API**: Core API for Fabric mods
- **Fabric Loader**: Mod loader runtime
- Additional utility mods (to be determined)

### Development Tools
- **Prism Launcher**: Primary testing platform for modpack imports
- **IntelliJ IDEA / VS Code**: Recommended IDEs for Java development
- **Gradle**: Build automation (use wrapper: `./gradlew`)

---

## Development Workflow

### Initial Setup (Phase 1)
1. Create the directory structure as outlined above
2. Initialize Fabric mod project in `/mod` using Fabric Loom template
3. Set Java version and verify compilation with `./gradlew build`
4. Ensure `.gitignore` excludes build artifacts and IDE files

### Adding Dependencies (Phase 2)
1. Download required mod JARs to `/external-mods`
2. Document all versions in `pack-meta/VERSIONS.md`
3. Never commit binary JARs to Git unless necessary (prefer Gradle dependency resolution)
4. Lock versions for reproducibility

### Custom Content (Phase 3)
1. **Fabric Mod Development**: Work in `/mod` using standard Fabric mod structure
2. **Datapacks**: Create in `/datapacks` with proper `pack.mcmeta`
3. **Configurations**: Place mod configs in `/config`, ensuring server/client compatibility
4. Test in a development Minecraft instance before integrating into build

### Build System (Phase 4)
The Gradle build system orchestrates the entire modpack assembly:

**Key Gradle Tasks**:
- `./gradlew :mod:build` - Compile custom Fabric mod
- `./gradlew prepareMods` - Collect mod JARs
- `./gradlew prepareDatapacks` - Package datapacks
- `./gradlew prepareConfigs` - Copy configuration files
- `./gradlew assemblePack` - Create final modpack ZIP

**Build Output**: `build/modpack-<version>.zip`

### Testing (Phase 5)
1. Import the generated ZIP into Prism Launcher
2. Verify mod load order and counts
3. Test world creation and gameplay
4. Confirm datapack activation
5. Check Cobblemon functionality
6. Test on both client and dedicated server (if applicable)

### Version Control Practices
```bash
# Feature development
git checkout -b feature/my-feature
# ... make changes ...
git add .
git commit -m "feat: descriptive commit message"
git push -u origin feature/my-feature

# Bug fixes
git checkout -b fix/issue-description
# ... fix bug ...
git commit -m "fix: description of fix"
```

---

## AI Assistant Conventions

### Code Style and Best Practices

#### Java/Fabric Mod Development
- **Java Version**: Use Java 17 or 21 features as appropriate
- **Naming Conventions**:
  - Classes: `PascalCase`
  - Methods/Variables: `camelCase`
  - Constants: `UPPER_SNAKE_CASE`
- **Package Structure**: `com.cobblerpg.[feature]`
- **Mixins**: Document clearly, use minimal injection points
- **Fabric API**: Prefer official APIs over reflection/access wideners

#### Gradle Scripts
- Use Kotlin DSL (`build.gradle.kts`) or Groovy DSL consistently
- Comment custom tasks with purpose and usage
- Pin dependency versions explicitly
- Use Gradle wrapper (`./gradlew`) exclusively

#### JSON/Configuration Files
- Use 2-space indentation
- Validate against Minecraft/mod schemas
- Include comments where format allows (JSON5 for configs)

#### Datapacks
- Follow Minecraft wiki conventions
- Namespace custom content properly
- Include README in complex datapacks
- Test with `/reload` command

### File Operations

#### When Creating New Files
- **Always verify location**: Check the intended directory structure
- **Use templates**: Reference existing Fabric mod templates for boilerplate
- **Update documentation**: Add new files to relevant README sections

#### When Modifying Files
- **Read first**: Always read a file before editing
- **Preserve formatting**: Match existing indentation and style
- **Minimize changes**: Only modify what's necessary for the task
- **Test after changes**: Run relevant Gradle tasks to verify

#### What NOT to Create
- ❌ Unnecessary abstraction layers (keep it simple)
- ❌ Premature optimization
- ❌ Documentation files unless requested
- ❌ Config files for features not yet implemented

### Task Management

#### Use TodoWrite For
- Multi-step implementations (3+ steps)
- Feature additions to the mod
- Build system modifications
- Testing and validation workflows

#### Don't Use TodoWrite For
- Single file edits
- Simple documentation updates
- Answering questions about the code

#### Task Breakdown Example
```
✓ Design feature structure
→ Implement core logic in mod/src
- Add configuration support
- Create datapack integration
- Update build.gradle if needed
- Test in development instance
- Update VERSIONS.md
```

### Common Operations

#### Adding a New Mod Dependency
1. Download the mod JAR to `/external-mods`
2. Document version in `pack-meta/VERSIONS.md`
3. If needed, add to `build.gradle` dependency resolution
4. Update the `prepareMods` Gradle task if custom handling needed
5. Test modpack assembly and import

#### Creating a New Fabric Mod Feature
1. Create Java classes in `mod/src/main/java`
2. Register with Fabric API initialization
3. Add any resources to `mod/src/main/resources`
4. Test with `./gradlew :mod:build && ./gradlew runClient`
5. Document feature in mod README

#### Modifying Cobblemon Configuration
1. Locate relevant config in `/config/cobblemon/`
2. Edit JSON carefully (validate syntax)
3. Test in game with `/reload` or restart
4. Document changes if significant

#### Building the Modpack
```bash
# Full build from clean state
./gradlew clean assemblePack

# Incremental rebuild (faster)
./gradlew assemblePack

# Build only the custom mod
./gradlew :mod:build
```

### Debugging Workflows

#### Mod Won't Load
1. Check `fabric.mod.json` for syntax errors
2. Verify Fabric API version compatibility
3. Check for mixin failures in logs
4. Ensure Java version matches target

#### Datapack Not Loading
1. Verify `pack.mcmeta` format
2. Check namespace conflicts
3. Use `/datapack list` in-game
4. Validate JSON syntax in data files

#### Build Fails
1. Check Gradle logs for specific errors
2. Verify all paths in build scripts
3. Ensure Java version is correct
4. Clear Gradle cache: `./gradlew clean --refresh-dependencies`

---

## Git Branch Strategy

### Branch Naming
- **Feature branches**: `feature/description` or `feat/description`
- **Bug fixes**: `fix/description`
- **Documentation**: `docs/description`
- **Build system**: `build/description`

### Protected Branches
- `main`: Stable, tested code only
- Develop on feature branches
- Use pull requests for code review (if team grows)

### Commit Message Format
```
<type>: <short description>

[optional body]

[optional footer]
```

**Types**:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only
- `build`: Build system/gradle changes
- `config`: Configuration file changes
- `datapack`: Datapack additions/modifications

**Examples**:
```
feat: add custom Pokémon spawn logic in Plains biome
fix: resolve crash when interacting with specific NPC
build: update assemblePack task to include server configs
config: increase Cobblemon spawn rates for rare Pokémon
datapack: add custom crafting recipes for Poké Balls
```

---

## Testing Checklist

Before considering a feature complete:

### Mod Development
- [ ] Code compiles without errors
- [ ] Mod loads in development environment
- [ ] No console errors during initialization
- [ ] Feature works as intended in-game
- [ ] No conflicts with Cobblemon or other mods

### Datapack Development
- [ ] JSON syntax validated
- [ ] Datapack loads with `/datapack list`
- [ ] Functions execute without errors
- [ ] No namespace conflicts
- [ ] Changes persist through `/reload`

### Build System
- [ ] `./gradlew assemblePack` completes successfully
- [ ] Generated ZIP has correct structure
- [ ] All required files present in ZIP
- [ ] ZIP size is reasonable (not bloated)
- [ ] Version numbers are correct

### Integration Testing
- [ ] Modpack imports into Prism Launcher
- [ ] Minecraft launches without crashes
- [ ] All mods load (check mod menu)
- [ ] World creation succeeds
- [ ] Gameplay features work
- [ ] Cobblemon features operational
- [ ] Custom content accessible

---

## Important Considerations

### Performance
- **Mod JARs**: Keep external mods up to date but test compatibility
- **Datapacks**: Minimize tick-heavy functions
- **World Generation**: Be cautious with custom biome modifications

### Compatibility
- **Minecraft Version**: Stick to one target version
- **Fabric Loader**: Match to Minecraft version
- **Mod Dependencies**: Verify mod compatibility matrix
- **Client vs Server**: Tag client-only or server-only mods appropriately

### Security
- ⚠️ **Mod Sources**: Only download mods from trusted sources (Modrinth, CurseForge)
- ⚠️ **Code Review**: Review any custom mixins or access wideners
- ⚠️ **Secrets**: Never commit API keys, tokens, or server credentials

### Versioning
- Use semantic versioning: `MAJOR.MINOR.PATCH`
- Document breaking changes in `VERSIONS.md`
- Tag releases in Git: `git tag v1.0.0`

---

## Useful Resources

### Official Documentation
- [Fabric Wiki](https://fabricmc.net/wiki/)
- [Cobblemon Wiki](https://wiki.cobblemon.com/)
- [Minecraft Wiki - Data Packs](https://minecraft.wiki/w/Data_Pack)
- [Prism Launcher Documentation](https://prismlauncher.org/wiki/)

### Gradle and Build Tools
- [Fabric Loom Documentation](https://github.com/FabricMC/fabric-loom)
- [Gradle User Guide](https://docs.gradle.org/current/userguide/userguide.html)

### Community Resources
- [Fabric Discord](https://discord.gg/v6v4pMv)
- [Cobblemon Discord](https://discord.gg/cobblemon)

---

## Quick Reference

### Essential Commands
```bash
# Build custom mod only
./gradlew :mod:build

# Assemble complete modpack
./gradlew assemblePack

# Clean build artifacts
./gradlew clean

# Run Minecraft client (development)
./gradlew :mod:runClient

# Run dedicated server (development)
./gradlew :mod:runServer

# Refresh dependencies
./gradlew --refresh-dependencies
```

### File Locations Quick Map
| Content Type | Location | Included in Pack |
|--------------|----------|------------------|
| Custom mod source | `/mod/src` | ✓ (compiled JAR) |
| Custom mod JAR | `/mod/build/libs` | ✓ |
| Third-party mods | `/external-mods` | ✓ |
| Datapacks | `/datapacks` | ✓ |
| Configurations | `/config` | ✓ |
| Version docs | `/pack-meta` | ✓ (optional) |
| Build output | `/build` | ✗ (ignored) |

---

## Project Status

**Current Phase**: Phase 1 - Project Setup (Planning)

Refer to `PLAN.md` for the complete development roadmap and phase checklist.

**Last Updated**: 2025-11-24

---

## Notes for AI Assistants

### Context Retention
- Always check `PLAN.md` for the current project phase
- Review `VERSIONS.md` before suggesting dependency updates
- Read existing code before proposing modifications
- Understand the Fabric mod structure before adding features

### When Uncertain
- Ask the user for clarification on Minecraft versions
- Verify mod compatibility before suggesting additions
- Confirm whether changes are for client, server, or both
- Check if feature should be in custom mod vs datapack

### Priorities
1. **Reproducibility**: Builds must be repeatable
2. **Simplicity**: Avoid over-engineering
3. **Compatibility**: Test with target Minecraft version
4. **Documentation**: Keep VERSIONS.md and README.md current

---

*This document is maintained for AI assistant context. Update it as the project evolves.*
