<div align="center">
  <img src="DictateOSS/Assets.xcassets/AppIcon.appiconset/icon_256x256.png" alt="DictateOSS icon" width="112" />
  <h1>DictateOSS</h1>
  <p>Native macOS dictation with local MLX Whisper and optional Groq acceleration.</p>

  <p>
    <img src="https://img.shields.io/badge/macOS-14%2B-000000.svg" alt="macOS 14 or newer" />
    <img src="https://img.shields.io/badge/Swift-SwiftUI-f05138.svg" alt="Swift and SwiftUI" />
    <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-2f6f65.svg" alt="MIT License" /></a>
  </p>
</div>

DictateOSS records from a global shortcut, transcribes speech locally or through Groq, optionally cleans up the text, and inserts the result into the active application.

```text
global shortcut → record audio → transcribe → optional text cleanup → paste
```

## Features

- Native menu bar application built with Swift and SwiftUI.
- Global shortcut for dictation from any macOS application.
- Local transcription through MLX Whisper.
- Optional Groq transcription and text cleanup using the user's API key.
- Local transcription history stored with SwiftData.
- Personal dictionary and replacement rules.
- Configurable language and formatting preferences.
- Recording overlay, audio feedback, microphone selection, and launch at login.
- Onboarding for microphone and Accessibility permissions.

## AI modes

| Mode | Transcription | Text cleanup | Intended use |
| --- | --- | --- | --- |
| Local | MLX Whisper | Off by default | Offline use and local processing |
| Groq | Groq | Groq | Faster setup with lower local resource use |
| Custom | Local or Groq | Off, Ollama, or Groq | Independent control of each stage |

Groq API keys are stored in macOS Keychain. When enabled, local fallback can continue the transcription flow if Groq is unavailable.

## Privacy

| Mode | Audio processing | Text cleanup | Persistent data |
| --- | --- | --- | --- |
| Local | On the Mac with MLX Whisper | Disabled by default or local through Ollama | History, dictionary, and replacement rules remain local |
| Groq | Sent to Groq for transcription | May be sent to Groq | Application data remains local; the API key is stored in Keychain |

Temporary audio files are deleted locally after processing. Local application data is stored on the Mac, but it is not presented as encrypted storage.

## Requirements

- macOS 14 or newer.
- Apple Silicon Mac for local MLX Whisper transcription.
- Xcode 16 or newer.
- XcodeGen.
- Python 3 and `mlx-whisper` for local mode.

Install the command-line dependencies:

```bash
brew install xcodegen
python3 -m pip install --user mlx-whisper
```

The default local model is `mlx-community/whisper-large-v3-turbo`. Other MLX Whisper presets can be selected in the application settings.

## Build and test

Generate the Xcode project:

```bash
xcodegen generate
```

Build from the command line:

```bash
xcodebuild build \
  -project DictateOSS.xcodeproj \
  -scheme DictateOSS \
  -destination 'platform=macOS'
```

Run the test suite:

```bash
xcodebuild test \
  -project DictateOSS.xcodeproj \
  -scheme DictateOSS \
  -destination 'platform=macOS'
```

You can also open `DictateOSS.xcodeproj` in Xcode after generating the project.

## Permissions

DictateOSS requires microphone access to record speech and Accessibility access to insert the resulting text into the active application.

The application runs outside the App Sandbox because the global dictation flow needs to monitor its shortcut, interact with the focused application, temporarily use the clipboard, simulate paste, and execute the locally installed `mlx_whisper` binary.

## Project status

DictateOSS is an early open-source application. The main dictation flow, provider selection, local history, dictionary, replacement rules, and settings are implemented. Packaging, signing, and local model setup may still require manual steps.

## License

Distributed under the [MIT License](LICENSE).
