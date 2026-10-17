# AGENTS.md

Instructions for every AI coding agent (Codex, Claude Code, Cursor, Gemini/Antigravity,
GitHub Copilot, Cline, rea, or any other) working in this repository.

---

## HARD RULE: build is fine, install/uninstall is never

**Building the APK is allowed. Installing, uninstalling, or clearing the app on any
device is forbidden.**

The owner of this fork stated this explicitly and permanently:

> "build boleh, tapi langsung install ke device itu dilarang sepenuhnya"

### Why — the one thing that actually destroyed data

Omalauncher keeps everything in the app's private `filesDir`
(`/data/data/dev.omalauncher/files`): the Arch Linux rootfs (`files/arch`), widgets,
themes, style, and settings. Android deletes `filesDir` on uninstall, with no backup.

A previous round uninstalled the installed **0.7.2** and installed a locally built
**0.8.0** debug APK. The uninstall wiped `filesDir`, and with it the owner's entire
Arch Linux system and all its data. That must never happen again.

The APK was never the problem. The device was.

### Forbidden — no exceptions, no "just to verify", no "the user probably wants it"

Installing or replacing the app:

- `./gradlew :app:installDebug` / `:app:installRelease`
- `adb install <any apk>`, `adb install-multiple`, `adb install --streaming`
- `adb push app-debug.apk <anywhere on the device>`

Uninstalling or wiping:

- `adb uninstall dev.omalauncher`
- `adb shell pm uninstall dev.omalauncher` (including `--user 0`)
- `adb shell pm clear dev.omalauncher` — same data loss as an uninstall
- `adb shell rm -rf /data/data/dev.omalauncher` or on its `files/` subdirs
- `adb shell rm -rf /sdcard/Android/data/dev.omalauncher`

Any CI job, Gradle task, script, or shell pipeline that ends in one of the above.

### Allowed — building (touches no device)

- `./gradlew :app:compileDebugKotlin`
- `./gradlew :app:lintDebug`
- `./gradlew :app:assembleDebug` / `:app:assembleRelease`
- `./gradlew :app:bundleDebug` / `:app:bundleRelease`
- `./gradlew build` — full build, also assembles the APK
- `./scripts/fetch-proot.sh` — build prerequisite (proot into `jniLibs`), no device

Build outputs stay in `app/build/`. They are never installed.

### Allowed — everything else

- Reading and exploring sources (`app/src/main/kotlin/**`, `owl/**`, `patches/**`, `docs/**`)
- Editing Kotlin / Compose / CMake / Gradle source files
- Static analysis, review, documentation
- `./gradlew :owl:test :app:testDebugUnitTest` — JVM unit tests, produces no APK
- `./gradlew :owl:owlJar` — the widget checker CLI, produces no APK
- Read-only device queries (`adb shell pm path dev.omalauncher`)
- Debug launches on an existing install (`adb shell am start -n dev.omalauncher/.MainActivity --es exec foot`)

Do not run `scripts/store-previews*.sh` or push a rootfs tarball unless asked —
they drive or feed the live app.

---

## Build prerequisites (reference — build allowed, install not)

| Tool | Version |
|---|---|
| JDK | 17 |
| Gradle | 9.8.0 (via `./gradlew`) |
| Android platform | `platforms;android-37.0` |
| NDK | `30.0.16248370` |
| SDK location | `local.properties` → `sdk.dir` |

`README.md` under "Building" describes upstream's workflow; its build commands are
fine, its `installDebug` step is not.

Full rule: `docs/BUILD-AND-INSTALL.md`.
