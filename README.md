# Dictator

**What you say goes... (to your cursor)**

Voice-to-text for Windows. Press a hotkey, speak, text appears at your cursor — in any app. No subscription. No account. No data leaves your machine. And if you want, your phone can be the microphone.

[**Download the latest release**](https://github.com/yuanbaca/Dictator-releases/releases/latest)

---

## Why Dictator?

- **Free.** No subscription, no account, no telemetry. The app runs on your machine, the models live on your disk, your voice data stays local.
- **Local-first by default.** Speech-to-text and AI formatting both run on your machine. The only cloud path is opt-in (Minimax TTS, your own API key, off by default).
- **Your phone is the microphone.** Unique to Dictator. Open the URL shown in the app on your phone's browser, tap mic, speak. Works over your WiFi or Tailscale. Nothing to install on the phone.
- **Portable single .exe.** No installer, no Python, no CUDA. Drop it anywhere and it keeps working.
- **Works wherever your cursor is.** Any app that accepts keyboard input — Notepad, Word, browsers, chat apps, IDEs, agentic coding tools. No plugins, no integrations, just typing.

---

## Two Ways to Dictate

### From your computer (hotkey)
Press `Ctrl+Shift+Space` (or your own hotkey), speak into your PC mic, press again. Text appears at your cursor.

### From your phone (wireless mic)
Open the URL shown in the app on your phone's browser. Tap the mic button and speak. Audio streams over your local network and text appears on your PC. No phone app — just a bookmark.

---

## Features

### Voice input
- **Speech-to-text** via [whisper.cpp](https://github.com/ggerganov/whisper.cpp) — runs offline, multiple model sizes from tiny to large
- **Configurable hotkeys** — record, inject text, read-aloud, and reformat each have their own global hotkey
- **Silence detection** — if you stop speaking mid-recording, the app shows a reminder with your stop hotkey. Threshold configurable (10–60s) or disable.
- **Progress beeps** during transcription, AI formatting, and cloud TTS loading so you know the app is working (toggleable under Sound Effects)
- **Escape** cancels recording or stops TTS playback — works globally, even when Dictator isn't focused

### AI text formatting
- **Optional local LLM** via [llama.cpp](https://github.com/ggerganov/llama.cpp) reformats your dictation before insertion. Downloads Phi-3 Mini (~2 GB) on first use.
- **10 format presets** — 3 cleanup tiers (Light / Clean / Strict), 2 email styles (Casual / Professional), Formal Letter, Bullet Summary, Meeting Notes, Documentation, Message
- **Fully customizable prompts** — edit the prompt behind any preset
- **Custom LLM models** — drop any GGUF file in `models/` and it auto-appears in Settings. Phi-3, Llama 3, and ChatML (Mistral/Qwen/Gemma) chat formats supported.
- **Highlight & reformat** — select text anywhere, press a hotkey, the LLM cleans it in place (Light / Clean / Strict)
- **System tray format picker** — left-click to toggle auto-format, right-click for the format list. Menu collapses to essentials when auto-format is off.

### Text-to-speech (read-aloud)
- **Local neural voices** via [Piper](https://github.com/rhasspy/piper) — 10+ downloadable models, custom `.onnx` voices supported
- **Windows SAPI** fallback — uses voices already on your system
- **Minimax cloud voices** *(optional, opt-in)* — 18 neural voices. Requires your own Minimax API key. Off by default. When enabled, TTS requests for Minimax voices go to Minimax; Piper and SAPI stay local.

### Phone companion
- **Any browser, no app install** — open the URL shown in Dictator, bookmark it, you're set
- **LAN** (same WiFi) or **Tailscale** (remote, trusted HTTPS) — pick whichever fits your setup

### Platform polish
- **Portable** — single .exe, models live next to it, move the folder, it keeps working. Self-repairs autostart registry when you move the exe.
- **GPU acceleration (beta)** — automatic [Vulkan](https://www.vulkan.org/) offload for Whisper and LLM. Falls back to CPU cleanly. Works on most modern NVIDIA/AMD/Intel discrete GPUs.
- **Free GPU toggle** — one click on the main screen unloads models from your GPU so games or other apps can use it
- **Auto-space** after each insertion (toggleable)
- **Single instance** — one copy at a time. A second launch tells you to check the tray.
- **Start minimized** — when autostarted, opens to the tray, not in your face
- **Dynamic tray menu** — "Cancel Recording" appears only while recording; format items appear only when auto-format is on
- **First-run model download** — no manual setup
- **Update check built in** — Settings → Check for Updates

---

## Minimum Hardware

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| **CPU** | Any 64-bit processor | 2015 or newer |
| **RAM** | 4 GB | 8 GB (if using AI formatting) |
| **Disk** | ~200 MB (app + Whisper model) | ~2.5 GB additional for AI formatting model |
| **GPU** | None required | Vulkan-compatible with 6+ GB VRAM (for faster transcription and formatting) |
| **OS** | Windows 10 | Windows 10/11 |

The base app (speech-to-text only) runs on modest hardware. AI text formatting loads a ~2 GB language model into RAM, so 8 GB is recommended if you enable it. GPU acceleration is automatic when available but not required.

---

## AI Models

Dictator uses local AI models by default — speech-to-text and AI text formatting both run on your machine, with no cloud calls and no API keys required. The only cloud integration today is optional: if you enable Minimax TTS in Settings with your own API key, text-to-speech requests for Minimax voices are sent to Minimax. Local Piper voices and Windows SAPI stay fully offline.

### Speech-to-text (Whisper)
The app downloads a Whisper model on first launch (~150 MB for the base English model). Larger models are available in Settings for better accuracy at the cost of speed.

### Text formatting (LLM)
When you enable auto-format, the app downloads a ~2 GB language model (Phi-3 Mini Q4). This model reformats your dictated speech — fixing grammar, removing fillers, structuring as emails, etc.

### Custom LLM models
You can use your own GGUF models:

1. Drop any `.gguf` file into the `models/` folder next to `dictator.exe`
2. The app auto-detects it and adds it to the Settings model list
3. Chat format is auto-detected from the filename, or override it in Settings

**Supported chat formats:**

| Format | Models | Auto-detected when filename contains |
|---|---|---|
| **Phi-3** | Phi-3, Phi-3.5 | `phi` (default if no match) |
| **Llama 3** | Llama 3, Llama 3.1 | `llama` |
| **ChatML** | Mistral, Qwen, Gemma, and many others | `mistral`, `qwen`, `gemma`, `chatml` |

**Tips:**
- Q4_K_M quantizations offer the best balance of speed and quality
- Smaller models (1–4B parameters) are faster but less accurate
- The app validates GGUF files before loading — corrupted or non-GGUF files are rejected

### Custom TTS voices
You can add your own Piper neural voice files:

1. Place a `.onnx` voice model and its matching `.onnx.json` config in the `models/piper-voices/` folder
2. They auto-appear in the Piper voices list in Settings with a "Custom" badge
3. Click "Use" to select, or "Delete" to remove

Find more Piper voices at [rhasspy/piper](https://github.com/rhasspy/piper/blob/master/VOICES.md).

### GPU Acceleration (beta)

Dictator automatically uses your GPU via [Vulkan](https://www.vulkan.org/) for both speech-to-text and AI text formatting. If a compatible GPU is detected, it's used automatically — no configuration needed. If not, the app falls back to CPU.

**Works well on:**
- NVIDIA GPUs with 6+ GB VRAM and recent drivers
- Most modern AMD and Intel discrete GPUs with current drivers
- Automatic CPU fallback if GPU initialization fails

**Known limitations:**
- **Low-VRAM GPUs** (≤4 GB) — the LLM alone needs ~3 GB of VRAM. GPUs without enough memory may fail to load the model. The app falls back to CPU, but you may see a brief delay during the failed attempt.
- **Integrated graphics only** (Intel UHD, Intel Iris) — most integrated GPUs lack the VRAM and compute for LLM inference. The app falls back to CPU.
- **Multi-GPU laptops** (NVIDIA Optimus) — the app may select the integrated GPU instead of the discrete one. If GPU performance seems slow, set Dictator to the high-performance GPU in Windows Graphics Settings.
- **GPU contention** — if your GPU is already under heavy load (gaming, rendering), there may not be enough VRAM. App should fall back to CPU, but you may notice slower startup.

**Troubleshooting:**
- Click **Free GPU** on the main screen — unloads models from the GPU and keeps Dictator on CPU until you toggle it back. Ideal if you want to start a game.
- Open Settings → enable **Force CPU mode** — same effect, persists across launches
- If you see "GPU disabled after previous crash," quit from the tray and relaunch, or click **Try GPU Again** in Settings

---

## Coming Next

The project's active focus areas. No ETAs — these ship when they ship.

- **GPU acceleration (beta) → GA** — finishing resilience work (sleep/resume, driver changes)
- **Custom dictionary / word bank** — teach the app words Whisper consistently mangles (e.g., "clod" → "Claude")
- **Transcription history** — local-only log of recent dictations, with one-click re-format without re-speaking
- **Live streaming transcription** — real-time text as you speak, instead of after you stop
- **More cloud TTS providers** — additional opt-in voice services beyond Minimax

---

## Requirements

- Windows 10 or 11
- Phone and PC on the same WiFi network (for phone dictation)
- Optional: [Tailscale](https://tailscale.com/) on both devices for remote access

## Getting Started

1. Download the latest `.zip` from the [Releases page](https://github.com/yuanbaca/Dictator-releases/releases/latest)
2. Extract anywhere on your PC (e.g., `C:\Tools\Dictator\`)
3. Run `dictator.exe`
4. The app downloads required models on first launch (~150 MB for speech-to-text)
5. Press `Ctrl+Shift+Space` to start dictating — or open the phone URL shown in the app

## Moving the App

Dictator is fully portable. If you move the exe to a new folder:

- **Models stay with the exe** — the `models/` folder next to `dictator.exe` is where downloaded models live. Move the whole folder together.
- **Start with Windows auto-repairs** — if you enabled autostart and then move the exe, Dictator detects the stale path on next launch and updates the registry automatically. No manual fix needed.
- **Manual fix (if needed)** — toggle "Start with Windows" off and back on in Settings. Or open the Registry Editor (`Win+R` → `regedit`) and update the path at:
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

1. **Before closing** — open Settings and turn off "Start with Windows" if enabled. This removes the autostart registry entry automatically.
2. **Close Dictator** — right-click the system tray icon → Quit
3. **Delete the app folder** — wherever you extracted the zip (contains `dictator.exe`, `models/`, `README.txt`)
4. **Delete app data** — Tauri stores webview cache here:
   ```
   %LOCALAPPDATA%\com.dictator.app
   ```
   Open File Explorer, paste that path in the address bar, and delete the folder.
5. **That's it** — Dictator doesn't install services, drivers, or modify system files.

If you already closed the app without disabling autostart, remove the registry entry manually:
- Press `Win+R`, type `regedit`, press Enter
- Navigate to: `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run`
- Delete the entry named `Dictator`

---

## Disclaimer

This project was built with AI assistance ([Claude](https://claude.ai/)). The app icon and visual assets are AI-generated art.
