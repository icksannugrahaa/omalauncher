# Claude Code instructions

**Build the APK freely. Never install, uninstall, or clear it on a device.**

Why: Android deletes `filesDir` on uninstall. That directory holds the Arch Linux
rootfs and all the owner's Linux data — a previous install round wiped it entirely.

Forbidden without exception: `./gradlew :app:installDebug|installRelease`, `adb install*`,
`adb install-multiple`, `adb push *.apk`, `adb uninstall dev.omalauncher`,
`adb shell pm uninstall|clear dev.omalauncher`, `adb shell rm -rf` on the app's storage.

Allowed: source reading/editing, static analysis, `:owl:test`, `:app:testDebugUnitTest`,
`:owl:owlJar`, and builds — `:app:compileDebugKotlin`, `:app:lintDebug`,
`:app:assembleDebug|Release`, `:app:bundle*`, `./gradlew build`, `./scripts/fetch-proot.sh`.

Read `AGENTS.md` at the repo root for the full rule and rationale.
