# androidtv-toolkit

Safe, revertable tweaks for Android TV devices, driven over `adb`.

The goal is to remove or disable unwanted apps and tweak settings **without ever making the stock build unrecoverable**. Every change is paired with a restore command.

## Principles

- Use `pm disable-user`, not `pm uninstall`, so the stock build stays revertable.
- Take a package list backup before any change.
- Record the matching restore command for every change in `RESTORE.md`.
- Never commit APKs, keystores, or backups that contain the TV's IP address.

## Layout

| Path | Purpose |
|---|---|
| `CLAUDE.md` | Rules for working on this repo |
| `RESTORE.md` | Restore command for every change made |
| `devices/` | One notes file per TV model |

## Current devices

- `MiTV-AXSO2`, Android TV 11 (see `devices/MiTV-AXSO2.md`)

## Requirements

- `adb` (Android platform-tools)
- `git`, and `gh` if you want to manage the repo from the CLI
