# Forza Motorsport 3 — agent guide

Portfolio rules: `E:\Source\dbce-project-mgmt\HOW-WE-WORK.md` (read first; it replaces older process, lease, review and testing instructions). Current focus: `E:\Source\dbce-project-mgmt\NOW.md`. Feature status lives in the portfolio grid (`python E:\Source\dbce-project-mgmt\tools\portfolio_grid.py plan fm3`), not in this file. Grid id: `fm3`.

## Project

Work-in-progress native Windows x64 recompilation of Forza Motorsport 3: Ultimate Collection (USA; Title ID `4D53084D`, `default.xex` 0.0.0.8) using the ReXGlue SDK (`thirdparty/rexglue-sdk`), Direct3D 12, with its own launcher. Remote `d-b-c-e/hl360-forza3` (visibility unconfirmed, so do not push). No game files in the repo; users supply both of their own discs.

## Build

From a terminal with `scripts\devenv.cmd` loaded (MSVC + Clang 20+): `scripts\build_sdk.cmd` once (applies `patches\sdk\*.patch`), then `scripts\build.cmd release`. Output in `out\build\win-amd64-release`. Extract Disc 1 into `game\disc1\` first (`scripts\install_game.cmd "<Disc 1.iso>"`). Generated code in `generated\default\` is rebuilt from `fm3_manifest.toml` and `config\*.toml`.

## Test

`launcher-regressions` (`tests/launcher_tests.cpp`, target `fm3-launcher-tests`) via `ctest --test-dir out\build\win-amd64-release`. User data and config sit next to `fm3.exe` (`fm3.user.toml`, `launcher.toml`) and under the user-data folder; back up and restore them around a run. Keep the user-data path under 96 characters.

## Install / run on the rig

Run `fm3.exe` or the launcher from the build output with the installed game; the launcher also has `--import-iso`, `--verify` and `--screenshot` modes. In game: F4 opens the SDK cvar editor, F3 the debug overlay.

## Shared code

No vendored DBCE toolkit copy found yet. Reusable code goes in the toolkit. Change shared code in the toolkit and re-sync; never edit the vendored copy.

## Project-specific rules

- `thirdparty/rexglue-sdk` has local changes; leave them alone unless the task is about the SDK.
- FM3 needs render readback; `readback_resolve = "fast"` stays (the `full` mode caps at 30 FPS).
- Do not commit game files, disc images, extracted assets or native binaries.
- Project Forza Plus 3 install backs up the original packages; uninstall must restore them.

## Key docs

- `README.md`, `CHANGELOG.md`
- `config/fm3.toml`
- `fm3_manifest.toml`


<!-- BEGIN project-file-location -->
## Project files and temporary work

Keep project-related research, evidence, scripts, packages, and other working files inside the relevant project folder whenever practical. Keep disposable or machine-specific outputs git-ignored when they do not belong in version control; never ignore files that should be committed merely to hide them. When a project folder is unsuitable, use `E:\Source\temp\<project>` instead of creating scattered drive-root folders. Record the location of material needed by later work and update references when relocating it.

Clean up temporary files and disposable copies promptly once they are no longer useful. Preserve active work, user data, settings, saves, required evidence, backups, rollback artifacts, and worktrees until their retention or replacement is established. Follow existing project, rig, and data-loss safeguards; do not remove files merely because they are old or untracked.
<!-- END project-file-location -->
