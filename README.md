# Remix Zoya Assistant 🎙️

A real-time, background-running multimodal voice assistant powered by Gemini Live Multimodal WebSocket API (`BidiGenerateContent`), built with modern Android, Jetpack Compose, and Kotlin Coroutines.

---

## 🚀 How to Build the APK

### Option 1: Automated GitHub Actions Build (Recommended)
This repository includes a preconfigured GitHub Actions workflow:
1. Push this repository to GitHub or fork it.
2. Navigate to the **Actions** tab on GitHub.
3. Select the **Build Android APK** workflow.
4. Click **Run workflow** (or simply push a commit).
5. Once completed, scroll down to the **Artifacts** section of the run and download `zoya-assistant-debug-apk.zip` containing `app-debug.apk`.

---

### Option 2: Local Command Line Build
Ensure you have **JDK 17** or higher installed and the **Android SDK** configured.

1. **Clone the repository:**
   ```bash
   git clone <repo-url>
   cd <repo-folder>
   ```

2. **Prepare the configuration:**
   Copy the example environment file:
   ```bash
   cp .env.example .env
   ```
   Add your Gemini API Key in `.env`:
   ```properties
   GEMINI_API_KEY=your_actual_gemini_api_key
   ```
   *(Note: You can also enter or change your API key directly in the app UI under Settings).*

3. **Restore Debug Keystore (if not already decoded):**
   ```bash
   base64 -d debug.keystore.base64 > debug.keystore
   ```

4. **Build the Debug APK:**
   ```bash
   ./gradlew assembleDebug
   ```

5. **Locate the APK:**
   The generated APK will be at:
   ```
   app/build/outputs/apk/debug/app-debug.apk
   ```

---

## 📱 Installation & Testing

1. **Transfer to Device:**
   Connect your Android phone via USB and run:
   ```bash
   adb install -r app/build/outputs/apk/debug/app-debug.apk
   ```
   *Or transfer `app-debug.apk` directly to your phone via Google Drive, WhatsApp, or USB and tap to install.*

2. **Required Permissions for Full Assistant Functionality:**
   - **Microphone:** Required for real-time 16 kHz duplex voice streaming.
   - **Notifications:** Required for the background persistent foreground service.
   - **Contacts & Phone:** Optional, required if you want Zoya to make calls or look up contacts by voice.
   - **Camera:** Optional, for torch/flashlight control.
   - **Accessibility Service:** Required for automated screen actions (Settings > Accessibility > Installed Apps > **Zoya Automation**).
   - **Modify System Settings:** Required for brightness adjustment (granted via app prompt).

---

## 🛠️ Tech Stack & Architecture
- **Language:** Kotlin 2.0+
- **UI:** Jetpack Compose with Material Design 3 and custom Canvas particle/glow orb animations
- **Audio Pipeline:** Low-latency `AudioRecord` (16kHz PCM) with hardware `AcousticEchoCanceler` and `NoiseSuppressor` + streaming `AudioTrack` (24kHz PCM)
- **Live AI Connection:** OkHttp WebSocket connecting to Gemini Live Multimodal API
- **Background Execution:** Android Foreground Service with type `microphone`
- **Build System:** Gradle Kotlin DSL (`.gradle.kts`) with Gradle Wrapper 9.3.1
