# AGENTS.md

Instructions for every AI coding agent (Codex, Claude Code, Cursor, Gemini/Antigravity,
GitHub Copilot, Cline, rea, or any other) working in this repository.

---

## HARD RULE: never build or install the APK

**Do not build the APK. Do not install the APK. Do not uninstall it either.**

The owner of this fork stated this explicitly and permanently:

> "jangan pernah lakukan build apk / re install apk omalauncher lagi"

### Forbidden — no exceptions, no "just to verify", no "user probably wants it"

- `./gradlew :app:assembleDebug`
- `./gradlew :app:assembleRelease`
- `./gradlew :app:bundleDebug` / `:app:bundleRelease`
- `./gradlew :app:installDebug` / `:app:installRelease`
- `./gradlew build` or any task that transitively assembles the `:app` APK/AAB
- `./scripts/fetch-proot.sh` (it exists only to feed the APK build)
- `adb install`, `adb install-multiple`, `adb push` of an APK
- `adb uninstall dev.omalauncher`, `adb shell pm uninstall`, `adb shell pm clear`
- Any CI job, Gradle task, script, or shell pipeline that ends in one of the above

### Allowed

- Reading and exploring sources (`app/src/main/kotlin/**`, `owl/**`, `patches/**`, `docs/**`)
- Editing Kotlin / Compose / CMake / Gradle source files
- Static analysis, review, documentation
- `./gradlew :owl:test :app:testDebugUnitTest` — JVM unit tests, produces no APK
- `./gradlew :owl:owlJar` — the widget checker CLI, produces no APK

### Why

A previous build replaced the owner's working install on their device: the installed
0.7.2 build was uninstalled and a locally built 0.8.0 debug APK was installed on top.
That destroyed their working state. Do not repeat it.

If the owner later asks for a build, treat this rule as still binding and require an
explicit, unambiguous reversal before acting.

---

## Build prerequisites (for reference only — do not run the build)

| Tool | Version |
|---|---|
| JDK | 17 |
| Gradle | 9.8.0 (via `./gradlew`) |
| Android platform | `platforms;android-37.0` |
| NDK | `30.0.16248370` |
| SDK location | `local.properties` → `sdk.dir` |

Documented in `README.md` under "Building". That section describes upstream's workflow,
not this fork's policy — the rule above overrides it.
