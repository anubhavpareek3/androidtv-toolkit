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

### Kept on purpose (not disabled)

Voice assistant (`com.google.android.katniss`), Chromecast (`com.google.android.apps.mediashell`), firmware updates, Google sign-in (`com.google.android.tvrecommendations`), TalkBack, Netflix, Prime Video, YouTube, Hotstar, ZEE5 (`com.graymatrix.did`), Airtel Xstream, FanCode.

### Notes

- Home is `com.google.android.tvlauncher`; PatchWall is not a Home handler on this TV (checked before disabling).
- Reference list used: `examples/xiaomi-mitv-axso2-android11/debloat.txt` from the cleanyourtv repo.
