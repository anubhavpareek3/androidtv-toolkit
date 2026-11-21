# RESTORE.md

Restore command for every change made to a TV. Entries are written **before** the change is applied.
Package list backup taken first: `backups/packages-all.txt` (local only, git-ignored).

To restore any package: `adb -s <tv-ip>:5555 shell pm enable <package>`

## 2026-11-21: MiTV-AXSO2 (Android TV 11, firmware RTT0.211201.001.1150)

Already disabled before we started (not ours, leave as is): `com.google.android.videos`, `com.google.android.play.games`.

Everything below was disabled with `pm disable-user --user 0 <package>`. Nothing was uninstalled.

### Group 1: telemetry

| Package | Restore |
|---|---|
| com.miui.tv.analytics | `pm enable com.miui.tv.analytics` |
| com.xiaomi.statistic | `pm enable com.xiaomi.statistic` |
| com.google.android.feedback | `pm enable com.google.android.feedback` |
| com.xiaomi.tvqs.overseas | `pm enable com.xiaomi.tvqs.overseas` |

### Group 2: promo rows, partner installs, PatchWall

| Package | Restore |
|---|---|
| com.mitv.tvhome.michannel | `pm enable com.mitv.tvhome.michannel` |
| com.mitv.tvhome.mitvplus | `pm enable com.mitv.tvhome.mitvplus` |
| com.xm.webcontent | `pm enable com.xm.webcontent` |
| com.xiaomi.floatingframe | `pm enable com.xiaomi.floatingframe` |
| android.autoinstalls.config.xioami.dangal | `pm enable android.autoinstalls.config.xioami.dangal` |
| com.google.android.partnersetup | `pm enable com.google.android.partnersetup` |
| com.mitv.tvhome.atv (PatchWall) | `pm enable com.mitv.tvhome.atv` |

### Group 3: preloads

| Package | Restore |
|---|---|
| com.xiaomi.mitv.tvmanager | `pm enable com.xiaomi.mitv.tvmanager` |
| com.xiaomi.mitv.handbook | `pm enable com.xiaomi.mitv.handbook` |
| com.xiaomo.tv.milegal | `pm enable com.xiaomo.tv.milegal` |
| com.xiaomi.mimusic2 | `pm enable com.xiaomi.mimusic2` |
| com.mitv.gallery | `pm enable com.mitv.gallery` |
| com.xiaomitv.sonyliv | `pm enable com.xiaomitv.sonyliv` |
| com.android.printspooler | `pm enable com.android.printspooler` |
| com.android.dreams.basic | `pm enable com.android.dreams.basic` |
| com.google.android.youtube.tvmusic | `pm enable com.google.android.youtube.tvmusic` |

### Group 4: apps and mirroring the owner doesn't use (owner's choice)

| Package | Restore |
|---|---|
| com.sonyliv (SonyLIV) | `pm enable com.sonyliv` |
| com.tv.sonyliv (second SonyLIV) | `pm enable com.tv.sonyliv` |
| com.zee5.xiaomiTV (old ZEE5 partner copy) | `pm enable com.zee5.xiaomiTV` |
| com.hungama.movies.xtv (Hungama) | `pm enable com.hungama.movies.xtv` |
| com.hoichoi.mitv (Hoichoi) | `pm enable com.hoichoi.mitv` |
| com.firestick.shemaroome (ShemarooMe) | `pm enable com.firestick.shemaroome` |
| app.siptv.android (SIPTV) | `pm enable app.siptv.android` |
| com.dsi.ant.plugins.antplus (ANT+ plugin) | `pm enable com.dsi.ant.plugins.antplus` |
| com.xiaomi.wfdsinkhelperservice (Miracast helper) | `pm enable com.xiaomi.wfdsinkhelperservice` |
| com.mitv.milinkservice (Mi Cast) | `pm enable com.mitv.milinkservice` |
| com.xiaomi.mitv.smartshare (Miracast/MiPlay receiver) | `pm enable com.xiaomi.mitv.smartshare` |

### Group 5: UNINSTALLED (owner approved `pm uninstall`, 2026-11-21)

Owner chose to uninstall these user-installed copies; no APK backup was kept (owner's choice). `pm enable` no longer applies to them. Restore = reinstall from the Play Store (the partner copies `com.xiaomitv.sonyliv`, `com.zee5.xiaomiTV` and `com.tv.sonyliv` may not be available there).

`com.sonyliv`, `com.tv.sonyliv`, `com.xiaomitv.sonyliv`, `com.zee5.xiaomiTV`, `com.hungama.movies.xtv`, `com.hoichoi.mitv`, `com.firestick.shemaroome`, `app.siptv.android`, `com.dsi.ant.plugins.antplus`

### Group 6: system apps removed for user 0 (owner approved, 2026-11-21)

`pm uninstall --user 0` on system apps; the files stay on the system partition, so these are restorable. They were already disabled before we started.

| Package | Restore |
|---|---|
| com.google.android.videos (Play Movies & TV) | `pm install-existing --user 0 com.google.android.videos` |
| com.google.android.play.games (Play Games) | `pm install-existing --user 0 com.google.android.play.games` |

### Group 7: custom launcher (cleanyourtv, package `com.gathin.tvhome`), INSTALLED 2026-11-21

Installed and set as Home; the `my-ambient-glass` theme is applied (theme undo: `tools\theme.ps1 -TvHost <tv ip> -Reset` from the work copy). APK 588,701 bytes, built with Gradle 8.14.2 from the cleanyourtv source.

Installing the launcher makes it Home and disables the stock launcher `com.google.android.tvlauncher`. To go back to stock Home:

```
adb shell pm enable --user 0 com.google.android.tvlauncher
adb shell cmd package set-home-activity --user 0 com.google.android.tvlauncher/.MainActivity
adb shell pm uninstall --user 0 com.gathin.tvhome        # optional: remove the custom launcher
```

The install script rolls back by itself if Home does not land on the new launcher. Theme: `themes/my-ambient-glass`. Revert the look only with `tools/theme.ps1 <theme> -Reset` (see the repo).

### Group 8: HDMI-CEC master switch (owner approved, 2026-11-21)

So a PS5 (One-Touch Play) switches the TV to its HDMI input by itself. Before: `hdmi_control_enabled=0`, `hdmi_control_auto_wakeup_enabled=0`, `hdmi_control_auto_device_off_enabled=0`. Only the master switch was changed; wake and device power-off links stay off by the owner's choice.

| Change | Restore |
|---|---|
| `settings put global hdmi_control_enabled 1` | `settings put global hdmi_control_enabled 0` |

### Test log: PatchWall re-enabled for the PS5 HDMI test (2026-11-21)

`com.mitv.tvhome.atv` (Group 2) was re-enabled with `pm enable --user 0 com.mitv.tvhome.atv` to see whether it handles the One-Touch-Play broadcast `droidlogic.tv.action.OTP_INPUT_SOURCE_CHANGED`. If it does not help, disable it again: `pm disable-user --user 0 com.mitv.tvhome.atv`. Result: no effect after a reboot with PatchWall running (the TV still did not switch to HDMI 2 when the PS5 woke; the Live TV app logs `mIgnoreHdmiCecTvInput: true`). PatchWall disabled again the same day.

### Theme change: launcher effects on (owner approved, 2026-11-21)

`themes/my-ambient-glass/config.json`: `app_glow`, `fade_rows`, `focus_overshoot` set from `false` to `true`. Restore: set them back to `false` and push the theme again, or undo the whole look with `tools\theme.ps1 -TvHost <tv ip> -Reset`.

### Theme change: ambient_glow on (owner approved, 2026-11-21)

`themes/my-ambient-glass/config.json`: `ambient_glow` set from `false` to `true`. Restore: set it back to `false` and push the theme again, or `tools\theme.ps1 -TvHost <tv ip> -Reset`.

### Theme change: advanced_effects on (owner approved, 2026-11-21)

`themes/my-ambient-glass/config.json`: `advanced_effects` set from `false` to `true` (heaviest effect, tested for RAM and frame drops). Restore: set it back to `false` and push the theme again, or `tools\theme.ps1 -TvHost <tv ip> -Reset`.

### Theme change: advanced_effects back off (owner's choice, 2026-11-21)

`advanced_effects` was turned on, then set back to `false` (frame stats were borderline: 85% janky in a 66-frame sample). To turn it on again, set it to `true` and push the theme.

### Theme change: stats line off (owner's choice, 2026-11-21)

`show_stats` set from `true` to `false` (the CPU/memory/storage line under the clock). Restore: set it back to `true` and push the theme.

### Launcher update: glass-pill app names (owner's choice, 2026-11-21)

Launcher rebuilt (589,145 bytes) with the app name as a small glass pill under the focused tile that fades in after about 0.45 s; theme sets `dock_title` to `false` (the big title above the dock is replaced by the pill). Previous APK (588,701 bytes) kept locally at `C:\Projects\work\apk-backup\app-release-before-pill.apk`. Roll back: `adb install -r` that APK, and set `dock_title` back to `true` in `themes/my-ambient-glass/config.json` and push the theme.

### Launcher update: pill fixes (owner's choice, 2026-11-21)

Pill widened (up to 1.7x the tile) so full names fit; the dock band now reaches down over the pill; YouTube (`com.google.android.youtube.tv`) is drawn as an icon tile (dark card) so it no longer looks like Netflix's white banner. APK 589,125 bytes. Previous APKs kept locally: `C:\Projects\work\apk-backup\app-release-pill-v1.apk` (pill v1) and `app-release-before-pill.apk` (original). Roll back: `adb install -r <apk>`.

### Launcher update: pill reverted, Netflix dark tile (owner's choice, 2026-11-21)

The glass-pill names looked odd, so they were reverted: the launcher is rebuilt from the original name behaviour and `dock_title` is back to the default (`true`, line removed from the theme). Netflix (`com.netflix.ninja`) is now the dark icon tile instead of YouTube (YouTube uses its own banner again). Local APK backups: `C:\Projects\work\apk-backup\app-release-before-pill.apk` (original), `app-release-pill-v1.apk`.

### Launcher update: Netflix back to its banner, name style (owner's choice, 2026-11-21)

`icon_tiles` is empty again (Netflix is its original white and red banner). The big name above the dock is now Outfit Extra Light and takes the focused app's own colour, lifted 35% toward white (`MainActivity.fillHero`, `activity_main.xml`). Roll back: `adb install -r C:\Projects\work\apk-backup\app-release-before-pill.apk` (original look), or the previous build is the one before this entry.

### Theme change: wallpaper rotation every 1 minute (owner's choice, 2026-11-21)

`wallpaper_rotation_minutes` set from `30` to `1` (the five gradient wallpapers cycle sequentially). Restore: set it back to `30` and push the theme.

### Theme change: ambient_glow back off (owner's choice, 2026-11-21)

`ambient_glow` set from `true` to `false` so the rotating gradient wallpapers show without the app colour tint. Restore: set it to `true` and push the theme.

### Theme change: photo wallpapers (owner's choice, 2026-11-21)

The five generated gradients in `themes/my-ambient-glass/wallpapers/` were replaced by 10 photos (landscapes, coast, space; Wikimedia Commons, credits in `wallpapers/CREDITS.md`), rotating every minute in order. No time-of-day groups. The gradient files are kept at `C:\Projects\work\gradients-backup\`. Restore: copy them back into `wallpapers/` (remove the photos) and push the theme, or `tools\theme.ps1 -TvHost <tv ip> -Reset`.

### Theme change: wallpaper dim 30 (owner approved, 2026-11-21)

`wallpaper_dim` set from `0` to `30` so the white clock, date and top buttons stay readable on bright photos. Restore: set it back to `0` and push the theme.

### Theme change: 30 wallpapers, name title off (owner's choice, 2026-11-21)

`themes/my-ambient-glass/wallpapers/` now holds 30 pictures: the 10 earlier photos, 15 more (city night, waterfalls, desert, aurora, sunset sky, forest, winter, ocean, Milky Way) and the 5 gradients, mixed in file-name order and rotating every minute. Credits in `wallpapers/CREDITS.md`. `dock_title` set to `false` (the big app name above the dock is hidden). Restore: remove `"dock_title": false` from `config.json`, delete the photos from `wallpapers/`, copy the gradients back from `C:\Projects\work\gradients-backup\`, push the theme; or `tools\theme.ps1 -TvHost <tv ip> -Reset`.

### Theme change: wallpaper order shuffle (owner's choice, 2026-11-21)

`wallpaper_order` set from `sequential` to `shuffle` (random pick each minute, never the same picture twice in a row). Restore: set it back to `sequential` and push the theme.

### Theme change: darker wallpapers (owner's choice, 2026-11-21)

Seven light wallpapers (brightness 132-183 of 255: Dune 45, Synevyr, Viru Bog winter, Thar dunes, Don Det pink sky, Teide, Kakerdaja fog) were replaced by seven darker ones (aurora and night-sky pictures, stormy sunset clouds). The removed pictures are kept at `C:\Projects\work\wallpapers-removed-light\`. Restore: copy them back into `wallpapers/`, delete the seven new files (`08-star-trails`, `12-aurora-tuntorp`, `17-virmalised-aurora`, `23-aurora-igloo`, `25-stormy-clouds-don-det`, `27-lofoten`, `29-cumulonimbus-sunset`) and push the theme.

### Launcher update: dock sits lower (owner's choice, 2026-11-21)

`MainActivity` pushes the dock row down by `AppAdapter.BOTTOM_ROOM_DP` (the room the launcher reserves under it, scaled by screen density) when `rows_below_dock` is 0, so the second row no longer peeks in at the bottom until you press Down. It works from the screen height, and stops 8 dp short of the bottom edge on short screens. Previous APK kept at `C:\Projects\work\apk-backup\app-release-name-colour.apk`. Roll back: `adb install -r` that APK.

### Theme change: black-hole wallpaper removed (owner's choice, 2026-11-21)

`30-black-hole-m87.jpg` (the blurry orange one) removed from `wallpapers/`, leaving 29 pictures. Restore: copy it back from `C:\Projects\work\wallpapers-removed-light\` into `wallpapers/` and push the theme.

### Theme change: more gradients, fewer cloudy photos (owner's choice, 2026-11-21)

Five cloudy photos removed from `wallpapers/` (`09-comet-atlas`, `10-vang-vieng-sky`, `12-aurora-tuntorp`, `25-stormy-clouds-don-det`, `29-cumulonimbus-sunset`; kept at `C:\Projects\work\wallpapers-removed-light\`) and ten generated dark gradients added (`31` to `40`, ember, emerald, rose, indigo-cyan, sunset, ocean, plum-gold, forest, ice, wine-coral). The folder now holds 34 pictures: 19 photos and 15 gradients. Restore: delete `31-` to `40-gradient-*.jpg`, copy the five photos back and push the theme.

### Kept on purpose (not disabled)

Voice assistant (`com.google.android.katniss`), Chromecast (`com.google.android.apps.mediashell`), firmware updates, Google sign-in (`com.google.android.tvrecommendations`), TalkBack, Netflix, Prime Video, YouTube, Hotstar, ZEE5 (`com.graymatrix.did`), Airtel Xstream, FanCode.

### Notes

- Home is `com.google.android.tvlauncher`; PatchWall is not a Home handler on this TV (checked before disabling).
- Reference list used: `examples/xiaomi-mitv-axso2-android11/debloat.txt` from the cleanyourtv repo.
