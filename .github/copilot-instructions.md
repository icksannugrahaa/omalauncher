# Repository rules

**Never build the APK. Never install or uninstall the APK.** No `assembleDebug`,
`assembleRelease`, `installDebug`, `bundle*`, `scripts/fetch-proot.sh`, `adb install`,
or `adb uninstall dev.omalauncher`.

Tests and the widget CLI are fine: `./gradlew :owl:test :app:testDebugUnitTest`,
`./gradlew :owl:owlJar`.

Full rationale and the complete forbidden/allowed list: see `AGENTS.md` at the repo root.
