# Repository rules

**Build the APK freely. Never install, uninstall, or clear it on a device.**

Uninstalling or `pm clear` deletes `filesDir`, which holds the Arch Linux rootfs and
all the owner's Linux data. It has destroyed their install once already.

Forbidden: `installDebug`, `installRelease`, `adb install`, `adb install-multiple`,
`adb push` of an APK, `adb uninstall dev.omalauncher`, `adb shell pm uninstall|clear
dev.omalauncher`.

Allowed: builds (`assembleDebug`, `assembleRelease`, `bundle*`, `./gradlew build`,
`scripts/fetch-proot.sh`), tests (`./gradlew :owl:test :app:testDebugUnitTest`), and the
widget CLI (`./gradlew :owl:owlJar`).

Full rationale and the complete forbidden/allowed list: `AGENTS.md` and
`docs/BUILD-AND-INSTALL.md` at the repo root.
