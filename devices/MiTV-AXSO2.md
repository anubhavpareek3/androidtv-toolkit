# MiTV-AXSO2

- Codename: dangalUHD (Amlogic), Android TV 11 (SDK 30)
- Firmware: RTT0.211201.001.1150, security patch 2024-02-05
- Home / launcher: `com.google.android.tvlauncher` (stock Android TV launcher)
- Reference list: `examples/xiaomi-mitv-axso2-android11/debloat.txt` in the cleanyourtv repo (not copied here)
- Storage: about 4.4 GB `/data`; tight. After cleanup and `pm trim-caches 64G`: about 2.1 GB free.

## Done (2026-11-21)

Package list backup taken first (local only, git-ignored). Every change and its restore command is in `RESTORE.md`.

- **Disabled (22), `pm disable-user`:** telemetry (Xiaomi analytics and statistics, Google feedback, Xiaomi quality uploader), PatchWall and promo rows (Mi Channel, Mi TV+, promo web container, floating overlay, partner auto-installs, partner setup), preloads (TV Manager, handbook, legal screens, Mi Music, Mi Gallery, print spooler, basic screensaver, YouTube Music), Miracast and Mi Cast mirroring.
- **Uninstalled (owner approved):** SonyLIV partner and duplicate copies, ZEE5 partner copy, Hungama, Hoichoi, ShemarooMe, SIPTV, ANT+ plugin.
- **Removed for user 0 (system apps, restorable):** Play Movies & TV, Play Games.
- **Kept on purpose:** voice assistant, Chromecast, Google sign-in (`tvrecommendations`), TalkBack, firmware updates, Live TV and inputs, Netflix, Prime Video, YouTube, Hotstar, ZEE5, Airtel Xstream, FanCode.

## Notes

- PatchWall (`com.mitv.tvhome.atv`) was not a Home handler on this TV, so disabling it was safe.
- The remote's PatchWall button does nothing now.
- Disabled packages stayed disabled after a reboot and after Play Store updates.
