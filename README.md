# Dictator

**What you say goes... (to your cursor)**

Voice-to-text for Windows — dictate from your computer with a hotkey, or from your phone as a wireless mic. Text appears wherever your cursor is. All processing happens locally on your machine. No cloud, no accounts, no data leaves your network.

[**Download the latest release**](https://github.com/yuanbaca/Dictator-releases/releases/latest)

---

## Features

- **Local speech-to-text** — powered by [whisper.cpp](https://github.com/ggerganov/whisper.cpp), no internet required. Multiple model sizes from tiny to large.
- **Desktop hotkey dictation** — press a hotkey, speak into your PC mic, press again. Text appears at your cursor in any app: Notepad, Word, browser, chat, IDE, anywhere.
- **Phone-as-wireless-mic** — open a URL on your phone, tap the mic button, speak. Your phone sends audio over your local network and text appears on your PC.
- **AI text formatting** — optional local LLM cleans up, rephrases, or reformats your dictation before insertion. Multiple presets:
  - **Cleanup** (3 tiers): Light cleanup (typos only), Clean up (grammar + fillers), Strict cleanup (full rewrite for clarity)
  - **Email**: Casual email, Professional email
  - **Other formats**: Formal letter, Bullet summary, Meeting notes, Documentation, Message
  - Fully customizable — edit the prompt behind any format preset
- **Custom hotkeys** — record, inject text, and read-aloud each have their own global hotkey. Choose from presets or set any key combination you want. Works from any app without switching windows.
- **Neural text-to-speech** — highlight text anywhere and hear it read aloud using [Piper](https://github.com/rhasspy/piper) neural voices or Windows SAPI. 10+ downloadable voice models.
- **System tray control** — left-click to toggle auto-format, right-click for format presets with grouped submenus
- **Text injection** via clipboard paste or simulated keystrokes
- **Auto-space** after each dictation (toggleable)
- **Progress beeps** — optional periodic chime during transcription and AI formatting so you know the app is working (toggleable under Sound Effects)
- **Escape key** cancels recording or stops TTS — works globally, even when Dictator isn't focused
- **Phone connection options**:
  - **LAN** (same WiFi) — works immediately
  - **Tailscale** — trusted HTTPS from anywhere, no browser warnings
- **Portable** — no installer, just an exe. Move it anywhere and it keeps working (see below).
- **First-run model download** — the app downloads what it needs on first launch, no manual setup
- **Check for updates** built in

## AI Models

Dictator uses local AI models — everything runs on your machine, nothing is sent to the cloud.

### Speech-to-text (Whisper)
The app downloads a Whisper model on first launch (~150 MB for the base English model). Larger models are available in Settings for better accuracy at the cost of speed.

### Text formatting (LLM)
When you enable auto-format, the app downloads a ~2 GB language model (Phi-3 Mini Q4). This model reformats your dictated speech — fixing grammar, removing filler words, structuring as emails, etc.

### Custom LLM models
You can use your own GGUF models for text formatting:

1. Drop any `.gguf` file into the `models/` folder next to `dictator.exe`
2. The app auto-detects it and adds it to the model list in Settings
3. Chat format is auto-detected from the filename, or you can override it in Settings

**Supported chat formats:**

| Format | Models | Auto-detected when filename contains |
|---|---|---|
| **Phi-3** | Phi-3, Phi-3.5 | `phi` (default if no match) |
| **Llama 3** | Llama 3, Llama 3.1 | `llama` |
| **ChatML** | Mistral, Qwen, Gemma, and many others | `mistral`, `qwen`, `gemma`, `chatml` |

**Tips:**
- Q4_K_M quantizations offer the best balance of speed and quality
- Smaller models (1-4B parameters) are faster but less accurate
- The app validates GGUF files before loading — corrupted or non-GGUF files are rejected

## In Progress

- **GPU acceleration** — graphics card support via Vulkan for faster transcription and LLM formatting. Currently functional as a build flag, being refined for automatic detection and seamless fallback.
- **Live streaming transcription** — real-time text appearing at your cursor as you speak, instead of waiting until you stop recording.

## Requirements

- Windows 10 or 11
- Phone and PC on the same WiFi network (for phone dictation)
- Optional: [Tailscale](https://tailscale.com/) on both devices for remote access

## Getting Started

1. Download the latest `.zip` from the [Releases page](https://github.com/yuanbaca/Dictator-releases/releases/latest)
2. Extract anywhere on your PC (e.g. `C:\Tools\Dictator\`)
3. Run `dictator.exe`
4. The app downloads required models on first launch (~150 MB for speech-to-text)
5. Press `Ctrl+Shift+Space` to start dictating — or open the phone URL shown in the app

## Moving the App

Dictator is fully portable. If you move the exe to a new folder:

- **Models stay with the exe** — the `models/` folder next to `dictator.exe` is where downloaded models live. Move the whole folder together.
- **Start with Windows auto-repairs** — if you enabled autostart and then move the exe, Dictator detects the stale path on next launch and automatically updates the registry to the new location. No manual fix needed.
- **Manual fix (if needed)** — if autostart stops working after a move, just toggle "Start with Windows" off and back on in Settings. Or open the Registry Editor (`Win+R` → `regedit`) and update the path at:
  ```
  HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run\Dictator
  ```

## Updating

1. Open Settings in Dictator and click **Check for Updates**
2. If an update is available, click **Copy link** — the download URL is copied to your clipboard
3. Paste the link in your browser and download the new zip
4. Close Dictator (tray icon → Quit)
5. Replace `dictator.exe` with the new one from the zip
6. Relaunch — your settings, models, and custom hotkeys are preserved

## Full Uninstall

Dictator is portable (no installer), so removing it means deleting the files it created:

1. **Close Dictator** — right-click the system tray icon → Quit
2. **Delete the app folder** — wherever you extracted the zip (contains `dictator.exe`, `models/`, `README.txt`)
3. **Delete app data** — Tauri stores webview cache here:
   ```
   %LOCALAPPDATA%\com.dictator.app
   ```
   Open File Explorer, paste that path in the address bar, and delete the folder.
4. **Remove autostart entry** (if you enabled "Start with Windows"):
   - Press `Win+R`, type `regedit`, press Enter
   - Navigate to: `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`
   - Delete the entry named `Dictator`
5. **That's it** — Dictator doesn't install services, drivers, or modify system files.

---

## Disclaimer

This project was built with AI assistance ([Claude](https://claude.ai/)). The app icon and visual assets are AI-generated art.
