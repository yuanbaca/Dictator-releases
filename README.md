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
- **Neural text-to-speech** — highlight text anywhere and hear it read aloud using [Piper](https://github.com/rhasspy/piper) neural voices or Windows SAPI. 10+ downloadable voice models.
- **Configurable global hotkeys** — record, inject text, and read-aloud each have their own hotkey. Works from any app without switching windows.
- **System tray control** — left-click to toggle auto-format, right-click for format presets with grouped submenus
- **Text injection** via clipboard paste or simulated keystrokes
- **Auto-space** after each dictation (toggleable)
- **Escape key** cancels recording or stops TTS — works globally, even when Dictator isn't focused
- **Phone connection options**:
  - **LAN** (same WiFi) — works immediately
  - **Tailscale** — trusted HTTPS from anywhere, no browser warnings
- **First-run model download** — the app downloads what it needs on first launch, no manual setup
- **Check for updates** built in

## In Progress

- **GPU acceleration** — graphics card support via Vulkan for faster transcription and LLM formatting. Currently functional as a build flag, being refined for automatic detection and seamless fallback.
- **Live streaming transcription** — real-time text appearing at your cursor as you speak, instead of waiting until you stop recording.

## Requirements

- Windows 10 or 11
- Phone and PC on the same WiFi network (for phone dictation)
- Optional: [Tailscale](https://tailscale.com/) on both devices for remote access

## Getting Started

1. Download the latest `.zip` from the [Releases page](https://github.com/yuanbaca/Dictator-releases/releases/latest)
2. Extract anywhere on your PC
3. Run `dictator.exe`
4. The app downloads required models on first launch (~150 MB for speech-to-text)
5. Press `Ctrl+Shift+Space` to start dictating — or open the phone URL shown in the app

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
