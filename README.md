<div align="center">
  <h1>HFW</h1>
  <p>An LSPosed/Vector module to rename apps, change icons, and hide apps across user profiles.</p>
  <p>
    <a href="https://github.com/everyoneexe/HFW/actions/workflows/build.yml"><img src="https://img.shields.io/github/actions/workflow/status/everyoneexe/HFW/build.yml?branch=main&label=Build&logo=github" alt="Build"></a>
    <a href="https://github.com/everyoneexe/HFW/releases/latest"><img src="https://img.shields.io/github/v/release/everyoneexe/HFW?label=Release" alt="Release"></a>
    <a href="https://github.com/everyoneexe/HFW/releases"><img src="https://img.shields.io/github/downloads/everyoneexe/HFW/total?label=Downloads" alt="Downloads"></a>
    <a href="LICENSE"><img src="https://img.shields.io/github/license/everyoneexe/HFW?label=License" alt="License"></a>
  </p>
</div>

---

## Features

HFW hooks `system_server` rather than modifying target APKs, so app data, signatures, and updates stay intact.

| Feature | Android | Scope |
|---|---|---|
| Rename apps | 8+ (8-10 untested) | Global and per-profile |
| Hide apps and Xposed modules | 11+ | Per-profile (8 configurable scopes) |
| Change app icons | 14+ (requires SELinux module) | Global and per-profile (including system apps) |
| Backup and restore | 8+ | Full `.zip` export and import of rules, labels, and icons |

- Supports Main, Work, Clone, and Private Space profiles.
- Detects Xposed modules automatically across classic and modern APIs, with per-app and bulk controls.
- No `INTERNET` permission; all state stays local on-device.

### Where Apps Can Be Hidden (Hide Scopes)

Each app can use the global defaults from Settings or its own custom hide mask:

- **Launcher & App Drawer** — removes the app from launchers and `LauncherApps` queries.
- **Suggestions** — hides the app from launcher/system app suggestions.
- **Shortcuts** — hides pinned and dynamic app shortcuts.
- **Shortcut Configs** — hides shortcut creation activities from widget/shortcut pickers.
- **Intent Chooser** — hides the app from "Open with" disambiguation dialogs.
- **Share Targets** — hides the app from the system share sheet.
- **Widgets** — hides the app's widgets from the widget picker.
- **Notifications** — suppresses notifications (and foreground-service listener visibility) from the app.

## Requirements

- Root with **LSPosed** or **Vector** (libxposed API 101 or newer).
- For custom icons on Android 14+: the bundled SELinux module (`hfw_sepolicy.zip`), exportable directly from the app.

## Installation

1. Install the HFW APK.
2. Enable **HFW** in LSPosed/Vector with scope set to **System Framework**, then reboot (or hot-reload on API 102).
3. *(Optional, for icon changes on Android 14+)* Open HFW, save the SELinux module zip, install it in your root manager (KernelSU / Magisk / APatch), and reboot.

## Notes

- System apps are hidden from the list by default and can be toggled in the sort menu.
- If you hide HFW itself from the launcher, open it again via **LSPosed/Vector -> Modules -> HFW**.
- Clearing HFW app data removes local icon previews (active overlays continue working until reset); export a `.zip` backup beforehand.

## Recovery (Boot Loop)

| Root Manager | Action |
|---|---|
| KernelSU | Press Volume Down 3+ times during boot to disable modules |
| Magisk | Boot into Safe Mode, or create `/data/adb/modules/<id>/disable` |
| APatch | Trigger Safe Mode via the power key during boot |

If needed, remove `/data/system/hfw/` to clear hook state.

## Build from Source

Requires **JDK 21** and **Android SDK (Platform 37)**.

```bash
./gradlew :app:verifyApi101Core :app:testDebugUnitTest
./gradlew assembleDebug assembleRelease
```

If you edit `sepolicy-module/`, rebuild `app/src/main/assets/hfw_sepolicy.zip` with `scripts/pack-sepolicy.sh`.

## Credits

Built with major contributions from [@MrRaita](https://github.com/MrRaita).

## Disclaimer

Provided for personal device customization and educational purposes; use at your own risk.

## License

Licensed under the [GNU General Public License v3.0](LICENSE).
