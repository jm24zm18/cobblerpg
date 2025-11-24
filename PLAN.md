🔧 Pull Plan — Fabric + Cobblemon Modpack Build System
🎯 Goal

Create a repeatable build system that produces a polished, installable modpack (zip) containing:

Your Fabric mod

Your datapacks

All third-party mods (Cobblemon, Fabric API, extras)

All configs

A clean structure Prism Launcher can import with one click

Version pinning + reproducible builds

🧱 Phase 1 — Project Setup
✔ 1. Create repository structure
cobblemon-pack/
  mod/
  datapacks/
  external-mods/
  config/
  pack-meta/
  build.gradle
  settings.gradle
  README.md

✔ 2. Initialize Git

Create main branch

Add .gitignore (Java, Gradle, Minecraft)

✔ 3. Set up Fabric mod in /mod

Use Fabric Loom template

Set Java version (17 or 21)

Build to confirm it outputs mod.jar

Deliverable: Repo builds without errors.

📦 Phase 2 — Dependency Capturing
✔ 4. Download third-party mods

Drop into:

external-mods/


Include at minimum:

Cobblemon

Fabric API

Other required libs

✔ 5. Create a version manifest

Add in pack-meta/:

VERSIONS.md


Document:

Minecraft

Fabric Loader

Fabric API

Cobblemon

All other mod versions

Deliverable: All mod versions locked and documented.

🛠️ Phase 3 — Datapack + Config Prep
✔ 6. Create your datapack folder
datapacks/my_custom_pack/
pack.mcmeta
data/<namespace>/...

✔ 7. Edit configs in /config

Tune Cobblemon spawn configs

Prepare any JSON modifications

Ensure configs match server/client compatibility

Deliverable: Datapacks + configs load without errors in a dev instance.

⚙️ Phase 4 — Gradle Build System Implementation
✔ 8. Configure root Gradle build

Add custom tasks:

prepareMods

prepareDatapacks

prepareConfigs

prepareMeta

assemblePack (builds final modpack zip)

✔ 9. Integrate mod project

Root Gradle must:

dependsOn(":mod:build")

Pull mod jar from /mod/build/libs

✔ 10. Output final modpack ZIP

Final ZIP structure should be:

mods/
config/
datapacks/
README.md
manifest.json (optional)

✔ 11. Add build instructions to README

Example:

./gradlew assemble


Deliverable: Running assemble produces a working modpack zip.

🧪 Phase 5 — Testing
✔ 12. Import ZIP into Prism Launcher

Use “Add Instance → Import from ZIP”

Confirm load order + mod counts

Test world load

Validate datapack loading

Verify Cobblemon functions

✔ 13. Test dedicated server mode (optional)

Export “server pack,” or

Copy modpack zip to server instance and run:

./run.sh

✔ 14. Validate reproducibility

Delete instance

Rebuild using new clone of the repo

Import again in Prism

Deliverable: Identical instances across machines.

🚀 Phase 6 — Optional Improvements
⭐ Add these when ready:

Client + server pack split

assembleClientPack

assembleServerPack

Automatic mod version checking

JSON manifest

Gradle task to verify JAR versions

Generate Modrinth .mrpack

Add packwiz or custom script

CurseForge export support

Add manifest.json builder task

GitHub Action CI

Auto-build pack zip on commit tag

🏁 Final Deliverables Checklist

✓ Repository with defined folder structure
✓ Fabric mod in /mod
✓ Datapacks in /datapacks
✓ Third-party mods in /external-mods
✓ Configs in /config
✓ Version manifest
✓ Gradle build system that exports modpack zip
✓ Modpack fully imports into Prism Launcher
✓ README with build instructions
✓ Optional: CI automation and platform exports

If you want, I can now:

✅ Write the full working build.gradle file
✅ Write the settings.gradle
✅ Build the /pack-meta/manifest.json
✅ Create the GitHub README template
✅ Add server/client pack support
