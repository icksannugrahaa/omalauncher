# GEMINI.md

**Building the Omalauncher APK is fine. Installing, uninstalling, or clearing it on a
device is never allowed.**

Why: an uninstall deletes `filesDir`, which holds the Arch Linux rootfs and all the
owner's Linux data. That has already destroyed their install once.

Forbidden: `./gradlew :app:installDebug`, `:app:installRelease`, `adb install`,
`adb install-multiple`, `adb push` of any APK, `adb uninstall dev.omalauncher`,
`adb shell pm uninstall|clear dev.omalauncher`, `adb shell rm -rf` on app storage.

Allowed: source reading/editing, static analysis, `:owl:test`, `:app:testDebugUnitTest`,
`:owl:owlJar`, and builds — `:app:compileDebugKotlin`, `:app:lintDebug`, `:app:assembleDebug`,
`:app:assembleRelease`, `:app:bundle*`, `./gradlew build`, `./scripts/fetch-proot.sh`.

Full rule and rationale: `AGENTS.md` and `docs/BUILD-AND-INSTALL.md`.
