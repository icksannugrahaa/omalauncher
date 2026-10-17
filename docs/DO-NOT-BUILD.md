# Do not build or install the APK

This fork has a standing rule from its owner:

> **Never build the APK. Never install or uninstall the APK.**

Applies to every agent and every human working in this repository.

## Forbidden

```sh
./gradlew :app:assembleDebug
./gradlew :app:assembleRelease
./gradlew :app:installDebug
./gradlew :app:installRelease
./gradlew :app:bundleDebug        # or bundleRelease
./gradlew build                    # transitively assembles the APK
./scripts/fetch-proot.sh
adb install app-debug.apk
adb uninstall dev.omalauncher
adb shell pm uninstall dev.omalauncher
adb shell pm clear dev.omalauncher
```

## Allowed

```sh
./gradlew :owl:test :app:testDebugUnitTest   # JVM tests, no APK
./gradlew :owl:owlJar                        # widget checker CLI
```

Plus ordinary source work: reading, editing Kotlin/Compose/CMake, static analysis,
review, documentation.

## Why

A previous local build replaced the owner's working install on their device — the
installed 0.7.2 build was uninstalled and a locally built 0.8.0 debug APK installed in
its place, destroying their working state. The owner does not want that repeated.

## Where this is recorded

| File | Read by |
|---|---|
| `AGENTS.md` | Codex, Cursor, Copilot, rea, and other AGENTS.md readers |
| `CLAUDE.md` | Claude Code |
| `GEMINI.md` | Gemini CLI / Antigravity |
| `.github/copilot-instructions.md` | GitHub Copilot |
| `.cursorrules` | Cursor |
| `docs/DO-NOT-BUILD.md` | humans and any agent that reads `docs/` |
| `.vscode/tasks.json` | VS Code task runner (build/install tasks removed) |

`README.md`'s "Building" section describes upstream's workflow. It is kept for
reference. The rule above overrides it for this fork.
