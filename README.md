# ShaNiu AI Smartphone Assistant

An intelligent voice assistant based on Android platform, activated by wake word, supporting voice recognition, voice output, and capable of executing general Q&A or automated phone operations (such as ordering food delivery) based on user commands.

## Features

- Voice Wake-up: Activate assistant through ShaNiu wake word (<500ms response)
- Voice Recognition: Real-time voice to text, supports streaming recognition and noise reduction
- Voice Synthesis: Customized ShaNiu voice, supports emotional voice synthesis
- Smart Dialogue: Multi-turn dialogue based on large language model, supports context memory
- Automated Operations: Automatically operate phone through accessibility service to complete tasks like ordering food, booking tickets
- Device Control: Open apps, adjust volume/brightness, toggle WiFi, etc.

## Tech Stack

- Development Language: Kotlin
- Android Version: API 31+
- Build Tools: Gradle 7.3.0, Kotlin 1.8.0
- Architecture: MVVM + Clean Architecture
- Voice Tech: Porcupine (wake-up) + Alibaba Cloud ASR/TTS
- AI Service: Tongyi Qianwen/GLM-4
- Accessibility Service: Android AccessibilityService

## Project Structure

```
app/
  src/main/java/com/fx/shaniu/
    module/           # Core modules
      wakeup/       # Voice wake-up
      asr/          # Voice recognition
      tts/          # Voice synthesis
      chat/         # Dialogue management
      autoglm/      # Automation operations
      device/       # Device control
    service/          # Android services
      WakeUpService.kt
      ShaniuAccessibilityService.kt
    manager/          # Managers
    ui/              # UI components
    data/            # Data layer
  src/test/            # Unit tests
  src/androidTest/      # Integration tests
  res/                 # Resource files
docs/
  DESIGN.md            # Detailed design document
```

## Quick Start

### Requirements

- Android Studio Arctic Fox or higher
- JDK 11 or higher
- Android SDK API 31+
- Gradle 7.3.0+

### Installation

1. Clone the repository
```bash
git clone https://github.com/your-org/shaniu.git
cd shaniu
```
2. Configure API keys

Add to `local.properties`:
```properties
# Alibaba Cloud ASR
alibaba.asr.app.id=your_app_id
alibaba.asr.app.key=your_app_key
# Alibaba Cloud TTS
alibaba.tts.app.id=your_app_id
alibaba.tts.app.key=your_app_key
# LLM API
llm.api.endpoint=your_llm_endpoint
llm.api.key=your_llm_key
```

3. Build project
```bash
./gradlew assembleDebug
```

4. Install to device
```bash
./gradlew installDebug
```

### Permissions

The following permissions are required on first launch:
- Recording permission (voice recognition)
- Accessibility service (automation operations)
- Overlay permission (system control)
- Modify system settings permission
- Location permission (location-related features)

For detailed configuration, see `docs/DESIGN.md` section 4.

## Usage

### Voice Wake-up

1. Say `ShaNiu` to wake up
2. Assistant responds: `I am here, please command`
3. Say your instruction: `What is the weather in Beijing today?`

### General Q&A

```
User: ShaNiu
Assistant: I am here, please command
User: Help me translate Hello World
Assistant: Hello World in Chinese means: Hello, World
```

### Automated Operations

```
User: ShaNiu
Assistant: I am here, please command
User: Help me order spicy hot pot to company
Assistant: OK, placing order for you...
[Automating Meituan Food Delivery]
Assistant: Order placed for spicy hot pot, estimated to arrive at company in 30 minutes
```

## Development Guide

### Build Commands

```bash
./gradlew assembleDebug       # Build Debug version
./gradlew assembleRelease     # Build Release version
./gradlew clean             # Clean build artifacts
./gradlew check             # Run all checks (tests + Lint)
```

### Run Tests

```bash
./gradlew test                              # Run unit tests
./gradlew connectedAndroidTest             # Run instrumented tests
./gradlew :app:test --tests "*ASRModuleTest*"  # Run specific module tests
```

### Code Style

Project follows Kotlin coding standards with 4-space indentation.

```bash
./gradlew lint    # Run Lint check
```

For detailed development guidelines, see `AGENTS.md`.

## Security

- All sensitive data is encrypted and stored using Android KeyStore
- Voice data is processed locally by default, not uploaded to cloud (requires user authorization)
- Sensitive operations like payment require manual secondary confirmation
- Accessibility service only processes whitelisted applications
- For complete security design, see `docs/DESIGN.md` section 5.

## Documentation

- [Detailed Design Document](docs/DESIGN.md) - Complete technical architecture and implementation plan
- [Contributor Guide](AGENTS.md) - Development standards and submission guidelines

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork this repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m "feat(asr): Add streaming support"`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Submit Pull Request

Please follow [Conventional Commits](https://www.conventionalcommits.org/) for commit messages.

## Roadmap

### Phase 1: Basic Features (In Progress)
- [x] Project architecture setup
- [ ] Voice wake-up module implementation
- [ ] Voice recognition module integration
- [ ] Voice synthesis module integration
- [ ] Basic UI development

### Phase 2: General Scenarios (Planned)
- [ ] Dialogue history management
- [ ] Context memory implementation
- [ ] Intent recognition optimization

### Phase 3: Automation Core (Planned)
- [ ] AccessibilityService development
- [ ] UI element locator implementation
- [ ] Action executor development
- [ ] Application adapter framework

### Phase 4: Scenario Expansion (Planned)
- [ ] More application adaptations
- [ ] Complex task orchestration

## License

This project is open sourced under [MIT License](LICENSE).

## Contact

- Project Homepage: https://github.com/your-org/shaniu
- Issue Tracker: https://github.com/your-org/shaniu/issues
- Email: your-email@example.com

---

**ShaNiu AI Smartphone Assistant** - Making your phone smarter!
