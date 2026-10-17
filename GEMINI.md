# GEMINI.md

**Never build, install, or uninstall the Omalauncher APK in this repository.**

Forbidden: `./gradlew :app:assembleDebug`, `:app:assembleRelease`, `:app:installDebug`,
`:app:bundle*`, `./gradlew build`, `./scripts/fetch-proot.sh`, `adb install`,
`adb uninstall dev.omalauncher`, `adb shell pm uninstall|clear dev.omalauncher`.

Allowed: source reading/editing, static analysis, `./gradlew :owl:test :app:testDebugUnitTest`,
`./gradlew :owl:owlJar`.

Full rule and rationale: `AGENTS.md` at the repo root.
