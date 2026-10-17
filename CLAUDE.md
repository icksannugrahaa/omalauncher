# Claude Code instructions

**Never build, install, or uninstall the Omalauncher APK here.**

Forbidden without exception: `./gradlew :app:assembleDebug|assembleRelease|installDebug|bundle*`,
`./gradlew build`, `./scripts/fetch-proot.sh`, `adb install*`, `adb uninstall dev.omalauncher`,
`adb shell pm uninstall|clear dev.omalauncher`.

Allowed: source reading/editing, static analysis, `./gradlew :owl:test :app:testDebugUnitTest`,
`./gradlew :owl:owlJar`.

Read `AGENTS.md` at the repo root for the full rule and rationale.
