# Build: allowed. Install/uninstall: never

This fork has a standing rule from its owner:

> **Building the APK is fine. Installing, uninstalling, or clearing the app on a
> device is forbidden. Never touch the owner's phone.**

Applies to every agent and every human working in this repository.

## The one thing that destroyed the data

This is not about the APK. It is about the device.

Omalauncher keeps all of its state in the app's private `filesDir`
(`/data/data/dev.omalauncher/files`): the Arch Linux rootfs (`files/arch`), widgets,
themes, style, state, and settings. **Android deletes that directory when the app is
uninstalled.** There is no backup and no recovery.

What happened before:

- The installed **0.7.2** build was uninstalled and a locally built **0.8.0**
  debug APK was installed in its place.
- The uninstall wiped `filesDir`: the entire Arch Linux system and everything the
  owner had inside it (`/home`, `pacman` packages, dotfiles, files in `~`) was gone.

The lesson: **anything that uninstalls, clears, force-replaces (different signing
key), or overwrites the app's data ends the owner's Linux install.** Reinstalling
Omalauncher does not bring any of it back.

## Forbidden — no exceptions

### Installing or replacing the app

```sh
./gradlew :app:installDebug
./gradlew :app:installRelease
adb install app-debug.apk               # any variant
adb install-multiple *.apk
adb install --streaming *.apk
adb push app-debug.apk /data/local/tmp  # or anywhere else on the device
```

Installing a locally built debug APK over a Play-signed install, or any APK signed
with a different key, requires an uninstall first — and that uninstall is exactly
what must never happen.

### Uninstalling or wiping

```sh
adb uninstall dev.omalauncher
adb shell pm uninstall dev.omalauncher
adb shell pm uninstall --user 0 dev.omalauncher
adb shell pm clear dev.omalauncher     # wipes filesDir + data, same loss
adb shell rm -rf /data/data/dev.omalauncher
```

`pm clear` is not a softer alternative: it deletes the same `filesDir` the
uninstall would.

### Removing the app's data by hand

```sh
adb shell rm -rf /data/data/dev.omalauncher/files/arch
adb shell rm -rf /sdcard/Android/data/dev.omalauncher
```

No `rm -rf` on the app's storage, ever, for any reason.

## Allowed — anything that does not touch the device

### Building

```sh
./gradlew :app:compileDebugKotlin          # quick compile check
./gradlew :app:lintDebug                   # lint, no APK
./gradlew :app:assembleDebug               # debug APK -> app/build/outputs/apk/debug/
./gradlew :app:assembleRelease             # release APK (unsigned/CI-signed)
./gradlew :app:bundleDebug                 # AAB
./gradlew :app:bundleRelease
./gradlew build                            # full build (also assembles the APK)
./scripts/fetch-proot.sh                   # build prerequisite: proot -> app/src/main/jniLibs
```

Build output stays on the development machine. Building never installs, never
touches a device, and is safe. Verifying compilation with `compileDebugKotlin`,
`assembleDebug`, or `./gradlew build` is encouraged before handing off code.

### Tests and tools

```sh
./gradlew :owl:test :app:testDebugUnitTest   # JVM tests, no APK
./gradlew :owl:owlJar                        # widget checker CLI
```

### Device reads and debug launches (read-only, non-destructive)

These do **not** replace, uninstall, or clear the app, so they leave the owner's
Linux install untouched:

```sh
adb shell pm path dev.omalauncher                          # read-only
adb shell dumpsys package dev.omalauncher | head -40        # read-only
adb push ArchLinuxARM-aarch64-latest.tar.gz /sdcard/Android/data/dev.omalauncher/files/
adb shell am start -n dev.omalauncher/.MainActivity --es exec foot   # debug launch, existing install
```

Caveats that make them still risky — don't run them unprompted:

- The `adb push` tarball is only consumed by the **first-run bootstrap** when
  `filesDir/arch` does not exist yet (`Bootstrap.obtainTarball`). Do not push it
  without being asked.
- `scripts/store-previews.sh` and `scripts/store-previews-demo.sh` drive the live
  app over `adb shell am start` to render previews and write `.demo-run.sh` into the
  guest's widget dir. They are screenshot tooling, not a per-change step.
- Any `adb shell pm` invocation ending in `install`, `uninstall`, or `clear` is
  forbidden — see above.

### Ordinary source work

Reading, editing Kotlin/Compose/CMake/Gradle, static analysis, review, and
documentation.

## Why a build is safe and an install is not

| Action | Reaches the device? | Can destroy `filesDir`? |
|---|---|---|
| `assembleDebug` / `build` | no | no |
| `fetch-proot.sh` | no | no |
| `installDebug` | yes | **yes** if it must replace a differently-signed build |
| `adb install` | yes | **yes** (same as above) |
| `adb uninstall` | yes | **yes, always** |
| `pm clear` | yes | **yes, always** |
| `adb shell am start` | yes | no |
| `adb shell pm path` | yes | no |

## Where this is recorded

| File | Read by |
|---|---|
| `AGENTS.md` | Codex, Cursor, Copilot, rea, and other AGENTS.md readers |
| `CLAUDE.md` | Claude Code |
| `GEMINI.md` | Gemini CLI / Antigravity |
| `.github/copilot-instructions.md` | GitHub Copilot |
| `.cursorrules` | Cursor |
| `docs/BUILD-AND-INSTALL.md` | humans and any agent that reads `docs/` |
| `.vscode/tasks.json` | VS Code task runner (only non-device tasks) |

`README.md`'s "Building" section describes upstream's workflow. It is kept for
reference. The install rule above overrides it for this fork.
