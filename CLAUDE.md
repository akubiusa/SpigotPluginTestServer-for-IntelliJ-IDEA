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
  - `ReBuild And Reload (Mac).run.xml` — shell run config that runs `.run/buildAndTest.sh`.
    Note: its pre-run Maven task currently references a run configuration named
    `MyMaid4 build`, which does not exist under `.run/` (only `Package build` does),
    so the dependency is stale — see "Working on this repository" below.
  - `ReBuild And Reload (Win).run.xml` — shell run config that runs `.run/buildAndTest.bat`;
    its pre-run task correctly references `Package build`.
- `.run/buildAndTest.sh` — macOS / Linux script. Intended workflow: prepare a Paper
  server directory, download Paper and mcrconapi, write `server.properties`, copy the
  built jar into `plugins/`, then reload via RCON (falling back to starting the server).
  Note: the existence checks that gate these steps are currently inverted (e.g.
  `[ -f "server/" ]`), so the script's real behavior diverges from this intent.
- `.run/buildAndTest.bat` — Windows counterpart of the same intended workflow (it uses a
  `run/` server directory rather than `server/`).
- `README.md` — short bilingual (Japanese / English) description.

## Working on this repository

- There is no build, test, or lint step for this repository itself. It is a set of
  configuration and script files.
- To verify a change, exercise the affected script/run configuration inside a real
  Spigot plugin project (the scripts assume a Maven `target/<plugin>.jar` output and
  a locally reachable Minecraft server on the RCON port).
- Keep `buildAndTest.sh` and `buildAndTest.bat` behaviorally in sync when editing one
  of them; they are two platform variants of the same intended workflow, but note they
  already diverge today (e.g. server directory `server/` vs `run/`, and different
  `PLUGIN_NAME`/`JAR_FILE` values).
- The `.run/*.run.xml` files use IntelliJ's `$PROJECT_DIR$` macro and reference each
  other by run-configuration name (e.g. the reload configs depend on a Maven build
  config). Preserve those names and macros when editing.

## Conventions

- The scripts expose `PLUGIN_NAME` / `JAR_FILE` variables that a consumer edits for
  their own plugin. `buildAndTest.bat` uses generic placeholder values
  (`PLUGIN-NAME` / `OUTPUT-JAR.jar`), but `buildAndTest.sh` currently hard-codes a
  specific plugin (`MyMaid4` / `MyMaid4.jar`). Treat these as edit points and do not
  hard-code unrelated project names.
- Shell scripts target `bash`; batch scripts target `cmd.exe`. Keep them portable to
  their respective platforms.

## Security

- The RCON password in the scripts (`rconpassword`) is a local test-server default,
  not a secret. Do not reuse it for anything exposed to a network, and never add real
  credentials to these scripts.

## Documentation updates

- Update `README.md` if the purpose or usage of the run configurations changes.
- Keep this file in sync when the `.run/` layout or the build/reload workflow changes.
