# CLAUDE.md

## Device

- TV: **MiTV-AXSO2**, Android TV 11.
- The stock build must stay revertable at all times.

## Rules

1. Only use `pm disable-user`. **Never** use `pm uninstall` unless the user explicitly approves it first.
2. Take a package list backup before any change (e.g. `adb shell pm list packages -f` saved under `backups/`).
3. For every change made, save the matching restore command in `RESTORE.md`.
4. Backups may contain the TV's IP address; keep them out of git (see `.gitignore`).
5. Show each command to the user before running it, and get approval before installs or system changes.
