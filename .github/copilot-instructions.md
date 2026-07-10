# Copilot code review instructions

This repository is a set of IntelliJ IDEA run configurations (`.run/*.run.xml`) and
helper scripts (`.run/buildAndTest.sh`, `.run/buildAndTest.bat`) for running a local
Bukkit / Spigot / PaperMC test server. It has no compiled source, tests, or CI.

## What to focus on in review

- **Shell / batch correctness.** These scripts do real filesystem and process work.
  Flag genuine bugs: existence checks with the wrong test operator (e.g. `-f` on a
  directory), inverted create-if-missing logic, missing quoting around paths, and
  incorrect exit/`errorlevel` handling.
- **Cross-platform parity.** `.run/buildAndTest.sh` (macOS/Linux) and
  `.run/buildAndTest.bat` (Windows) are intended to implement the same workflow, though
  they already diverge today (e.g. server directory `server/` vs `run/`, and
  `PLUGIN_NAME`/`JAR_FILE` values). Flag *new* changes that widen this behavioral gap
  (server directory, downloaded artifacts, RCON command, ports).
- **IntelliJ run-config integrity.** In `.run/*.run.xml`, flag broken XML, changed
  run-configuration names that break the reload → build dependency, or removed
  `$PROJECT_DIR$` macros.
- **Hard-coded project specifics.** This is meant to be reusable; flag hard-coded
  plugin names or jar names that should be placeholders/variables instead.

## What NOT to flag

- The default RCON password (`rconpassword`) and ports — these are intentional
  local test-server defaults, not leaked secrets.
- Absence of a build system, tests, or CI — this repo is configuration only.
- Downloading Paper / mcrconapi at runtime — that is the intended setup behavior.

## Conventions

- Keep shell scripts POSIX/bash-portable and batch scripts valid for `cmd.exe`.
- Prefer short, imperative script comments consistent with the existing files.
