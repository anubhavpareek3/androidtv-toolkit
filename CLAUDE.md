# CLAUDE.md

## Device

- TV: **MiTV-AXSO2**, Android TV 11. Details: `devices/MiTV-AXSO2.md`. State and next step: `SESSION.md`.
- The stock build must stay revertable at all times.
- Launcher work copy lives outside this repo (`C:\Projects\work\cleanyourtv`, not git). Upstream reference: `C:\Projects\reference\cleanyourtv` (read-only).
- This repo is **public**: everything committed is visible to everyone.

## Rules

1. Use `pm disable-user` by default. `pm uninstall` only after the user explicitly approves that package.
2. Take a package list backup before any change (`adb shell pm list packages -f`, saved under `backups/`).
3. Write the restore command in `RESTORE.md` **before** applying each change, including theme and launcher changes.
4. Show each command before running it. Get approval before installs and system changes.
5. Never commit, push or change repo visibility without the user's approval. Commit email is the GitHub noreply address.
6. Never put the TV IP, Wi-Fi name, serials, personal email or keys in files: use `<tv ip>`.
7. Keep out of git: backups, APKs, keystores, and `tools\adbkey` (a private key). See `.gitignore`.
8. Any test that can cut the connection (Wi-Fi off) must revert itself on the TV, e.g. `setsid sh -c '...; sleep 25; ...'`.

## Working notes

- Theme changes need no rebuild: `tools\theme.ps1 -TvHost <tv ip> themes\my-ambient-glass`. `-Reset` removes the theme.
- Screenshots: use bash `adb exec-out screencap -p > file`. PowerShell `>` corrupts PNGs.
- Build: JDK 17 plus Gradle 8.14.2. If a file lock appears, run `gradle --stop` and retry.
- Keep numbers in docs exact (counts of packages, wallpapers). Check them before writing.
- Keep replies short. Ask when a choice is the user's (look, apps to remove).
