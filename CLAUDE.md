# CLAUDE.md

## Overview

This repository provides IntelliJ IDEA run configurations and helper scripts for
spinning up a local Bukkit / Spigot / PaperMC server to test a plugin under
development. It is not an application with its own build; the `.run/` files are
meant to be used from inside a Maven-based Spigot plugin project (one that builds
a jar into `target/`), where they build the plugin and hot-reload it on a running
Paper test server via RCON.

## Repository layout

- `.run/*.run.xml` — IntelliJ IDEA run configurations (`ProjectRunConfigurationManager`):
  - `Package build.run.xml` — Maven `clean package` run configuration.
  - `ReBuild And Reload (Mac).run.xml` — shell run config that runs `buildAndTest.sh`.
  - `ReBuild And Reload (Win).run.xml` — shell run config that runs `buildAndTest.bat`.
- `.run/buildAndTest.sh` — macOS / Linux script: prepares a Paper server directory,
  downloads Paper and mcrconapi, writes `server.properties`, copies the built jar
  into `plugins/`, then reloads via RCON (falling back to starting the server).
- `.run/buildAndTest.bat` — Windows equivalent of the above.
- `README.md` — short bilingual (Japanese / English) description.

## Working on this repository

- There is no build, test, or lint step for this repository itself. It is a set of
  configuration and script files.
- To verify a change, exercise the affected script/run configuration inside a real
  Spigot plugin project (the scripts assume a Maven `target/<plugin>.jar` output and
  a locally reachable Minecraft server on the RCON port).
- Keep `buildAndTest.sh` and `buildAndTest.bat` behaviorally in sync when editing one
  of them; they are two platform variants of the same workflow.
- The `.run/*.run.xml` files use IntelliJ's `$PROJECT_DIR$` macro and reference each
  other by run-configuration name (e.g. the reload configs depend on a Maven build
  config). Preserve those names and macros when editing.

## Conventions

- The scripts contain project-specific placeholders (`PLUGIN_NAME`, `JAR_FILE`) that
  a consumer edits for their own plugin. Do not hard-code unrelated project names.
- Shell scripts target `bash`; batch scripts target `cmd.exe`. Keep them portable to
  their respective platforms.

## Security

- The RCON password in the scripts (`rconpassword`) is a local test-server default,
  not a secret. Do not reuse it for anything exposed to a network, and never add real
  credentials to these scripts.

## Documentation updates

- Update `README.md` if the purpose or usage of the run configurations changes.
- Keep this file in sync when the `.run/` layout or the build/reload workflow changes.
