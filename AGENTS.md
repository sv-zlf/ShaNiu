# Repository Guidelines

## Project Structure & Module Organization

The project follows a modular architecture with clear separation:

- **app/src/main/java/com/fx/shaniu/**: Core application code
  - `module/`: Core modules (wakeup, asr, tts, chat, autoglm, device)
  - `service/`: Android services (WakeUpService, ShaniuAccessibilityService)
  - `manager/`: Management and utility classes
  - `ui/`: UI components and activities
  - `data/`: Data layer (repository, local, remote)
- **app/src/test/**: Unit tests
- **app/src/androidTest/**: Integration tests
- **docs/**: Design documents and technical specifications
- **app/src/main/res/**: Android resources (layouts, values, XML configs)

## Build, Test, and Development Commands

Build debug: ./gradlew assembleDebug
Build release: ./gradlew assembleRelease
Clean: ./gradlew clean
Run tests: ./gradlew test
Run instrumented tests: ./gradlew connectedAndroidTest
Run all checks: ./gradlew check
Install to device: ./gradlew installDebug

## Coding Style & Naming Conventions

Primary language: Kotlin. Android API: 31+. Use 4-space indentation.

Naming:
- Classes: PascalCase (WakeUpService, ASRModule)
- Functions: camelCase (startListening, locateElement)
- Constants: UPPER_SNAKE_CASE (WAKE_WORD, MAX_RETRY_COUNT)
- Files: PascalCase (MainActivity.kt)

Use ktlint for formatting. Run: ./gradlew lint

## Testing Guidelines

Frameworks: JUnit 5, AndroidX Test, MockK, kotlinx-coroutines-test.

Coverage: Aim for 70% overall. Critical modules (autoglm, accessibility) should exceed 85%.

Test files: ClassNameTest.kt (e.g., ASRModuleTest.kt)
Test names: should[ExpectedBehavior]When[StateUnderTest]()
Example: shouldRecognizeVoiceSuccessfullyWhenAudioInputIsClear()

Commands:
All tests: ./gradlew test
Specific module: ./gradlew :app:test --tests "*ASRModuleTest*"
Instrumented: ./gradlew connectedAndroidTest

## Commit & Pull Request Guidelines

Follow conventional commits: <type>(<scope>): <subject>

Types: feat (feature), fix (bug), docs, style, refactor, test, chore.

Examples:
feat(asr): Add streaming voice recognition support
fix(autoglm): Resolve UI element location failure
docs(readme): Update installation instructions

Before submitting PR:
- Update documentation
- Run ./gradlew check
- Run ./gradlew lint and fix warnings
- Add tests for new features
- Rebase on latest main branch

PR requirements:
- Clear title and description
- Explain problem and solution
- Screenshots/videos for UI changes
- Link related issues (e.g., Fixes #123)
- Checklist of completed items

PR naming: Use issue number prefix when possible: #123-Brief-description

## Security & Configuration

Never commit API keys. Store secrets in local.properties or environment variables.

Use Android KeyStore for user data. Encrypt voice data before transmission. Implement data masking for PII (phone numbers, cards).

Accessibility service: Process only whitelisted apps. Log operations for audit. Never expose accessibility data externally.

## Architecture Overview

Four-layer architecture:
1. Presentation (ui/): Activities, fragments
2. Business Logic (module/): Voice, chat, automation
3. Data (data/): Repositories, storage, remote services
4. System (service/): Background Android services

Patterns: MVVM (ViewModel + LiveData/Flow), Clean Architecture, Singleton (core modules), Adapter (app automation), Repository (data access).
