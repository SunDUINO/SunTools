# SunGopher AI for Android

[Wersja polska](ANDROID.PL.md)

The mobile version of the SunGopher AI agent. Same Go backend and same interface as the desktop app, adapted to a phone screen, with conversation sync to your computer over the home network.

> The app interface is in Polish. UI labels are quoted below in Polish with an English translation.

---

## Contents

- [Requirements](#requirements)
- [Installation](#installation)
- [First launch](#first-launch)
- [Using the app](#using-the-app)
- [Syncing with your computer](#syncing-with-your-computer)
- [Data and privacy](#data-and-privacy)
- [Troubleshooting](#troubleshooting)
- [Building from source](#building-from-source)

---

## Requirements

| | |
|---|---|
| OS | Android 8.0 or newer |
| CPU | ARM64 (`arm64-v8a`) — virtually every phone from recent years |
| Internet | required to talk to AI models |
| API key | at least one: OpenAI, Gemini, Anthropic or OpenRouter |

The Android version supports **online models only**. Local Ollama is available on desktop only.

---

## Installation

### Option 1 — APK on the phone

1. Download `SunGopher_AI-android-*.apk`.
2. Open the downloaded file on your phone (from the notification or the *Files* app).
3. If Android asks for permission to **install unknown apps**, allow it for the browser or file manager you're opening the APK from.
4. Tap **Install**.

> **Google Play Protect** may warn that the app is unknown — this is normal for apps from outside the Play Store. Choose **More details → Install anyway**.

### Option 2 — via `adb` from a computer

1. Enable **Developer options** on the phone: *Settings → About phone →* tap **Build number** 7 times.
2. In *Developer options* turn on **USB debugging** and connect the phone with a cable.
3. Install Platform-Tools (Windows: `winget install Google.PlatformTools`) and run:

```bash
adb devices                                   # accept the prompt on the phone
adb install -r SunGopher_AI-android-debug.apk
```

### Updating

Install the newer APK over the existing one (or `adb install -r`). **API keys, conversations and computer pairing are kept.** Uninstalling the app removes all of its data.

---

## First launch

1. Start **SunGopher AI**.
2. Tap **⚙** in the top-right corner.
3. Enter your name (optional) and the **API key** for at least one engine.
4. Tap **Zapisz** (*Save*).
5. Open the **☰** menu and select the AI engine you entered a key for.

API keys are configured **separately on each device** — they are not transferred during sync.

---

## Using the app

### Main screen

| Element | Action |
|---|---|
| **☰** | slide-out menu: new chat, AI engine selection, saved conversations |
| **⚙** | settings: API keys, sync with computer |
| **📎** | attach a file or image to the message |
| **📷** | take a photo and attach it to the message |
| **👁️** | live view — the agent can look through the camera on its own |
| **Wyślij / ⏹ Stop** | send the message / stop generating |

Picking a conversation or **+ Nowy czat** (*New chat*) in the ☰ menu closes the menu automatically. Delete a conversation with the **✕** next to its name.

### Camera and the Vision skill

- **📷 Photo** — opens the camera full screen. **📸 Zrób zdjęcie** (*Take photo*), then **Użyj zdjęcia** (*Use photo*) or **Ponów** (*Retake*). The photo is attached to your next message.
- **🔄** — switches between front and back camera. The back camera is the default.
- **Agent asks for a photo** — when the model decides it needs to see something, the camera opens with a note on what it needs. Send a photo or tap **Odmów** (*Decline*).
- **👁️ Live view** — a small camera thumbnail appears in the corner. While it's on, the agent may grab frames **without asking** (each grabbed frame flashes a white border). Drag the thumbnail by its bar to move it. **✕** turns off the live view and the camera.

On first camera use Android asks for permission — choose **Allow**.

Photos are downscaled to 1024 px on the longer side before sending: faster over the network and fewer tokens used.

---

## Syncing with your computer

Conversations sync with the desktop version of SunGopher AI (v1.10.00+) when **the phone and computer are on the same home network**.

### Pairing (once)

1. **On the computer:** ⚙ Settings → **Synchronizacja z telefonem** (*Sync with phone*). Leave the QR code on screen.
2. **On the phone:** ⚙ Settings → **📷 Skanuj kod QR z komputera** (*Scan QR code from computer*).
3. Point the camera at the QR code. The phone reads it, finds the computer on the network and syncs right away.
4. The computer's address with a padlock **🔒** appears in the phone's settings — pairing done.

> **Windows:** on first launch of SunGopher AI the firewall asks for network access. Allow it for **private networks** — sync uses port **8766**.

**Without a camera:** expand **Wpisz adres ręcznie** (*Enter address manually*), type in the address and code shown on the computer below the QR code, then tap **Zapisz i synchronizuj** (*Save and sync*).

### Day to day

- The phone syncs **automatically** when the app starts and every time you return to it (at most every 30 seconds).
- Away from home sync silently does nothing — there's nothing to turn off.
- **Synchronizuj teraz** (*Sync now*) in Settings forces a sync and shows how many messages were received and sent.
- On the computer the conversation list refreshes when you return to the app window.
- **Deleting a conversation** on one device deletes it on the other as well.

### Security

- Sync works on the local network only, with no intermediate servers.
- The connection is encrypted with **TLS 1.3**. The computer has its own certificate, and its SHA-256 fingerprint is embedded in the QR code — the phone will only connect to the paired computer.
- An access code from the computer is required as well.
- The desktop app UI still listens on `127.0.0.1` only; only the sync port is reachable on the network.

---

## Data and privacy

- Conversations, settings and API keys are stored **only in the app's storage on the phone** (and on the paired computer after syncing).
- Conversation content and photos are sent to the AI provider you choose (OpenAI, Google, Anthropic, OpenRouter), subject to its policies.
- The app's built-in server listens on `127.0.0.1` only and accepts connections exclusively from the SunGopher AI window — other apps on the phone can't access it.
- Uninstalling the app removes all of its data from the phone.

---

## Troubleshooting

| Problem | Solution |
|---|---|
| *"nie znaleziono modelu lub adresu API"* (model or API address not found) | Check the API key in ⚙. If the key is correct, close the app (swipe it away from recent apps) and start it again. |
| QR code not recognised | Move closer so the code fills more of the view, let it focus, reduce glare. Or use manual entry. |
| *"kod odczytany, ale komputer nie odpowiada"* (code read but computer not responding) | Phone and computer must be on the same Wi-Fi. Check that SunGopher AI is running on the computer and that the firewall allows port 8766 (private network). Guest networks often block traffic between devices. |
| *"komputer odrzucił kod synchronizacji"* (computer rejected the sync code) | The code on the computer has changed — pair the phone again via QR. |
| *"certyfikat komputera się nie zgadza"* (computer certificate mismatch) | The computer has a new certificate (e.g. after reinstalling). Scan the QR code again. |
| Camera doesn't work | *Android Settings → Apps → SunGopher AI → Permissions → Camera → Allow*. |
| App shows *"Backend Go nie wystartował"* (Go backend failed to start) | Collect logs with `adb logcat -s gophersun` and report it in [Issues](../../issues). |

---

## Building from source

Linux (recommended) with:

- Go (version from `go.mod`),
- JDK 17 or 21,
- Gradle 8.10+,
- Android SDK: `platforms;android-35`, `build-tools;35.0.0`, `platform-tools` (**no NDK needed**).

```bash
export ANDROID_HOME=~/Android/Sdk
go mod tidy
bash build_android.sh            # output: bin/SunGopher_AI-android-debug.apk
INSTALL=1 bash build_android.sh  # + install via adb
```

### How it works

```
┌──────────────── APK ────────────────┐
│ MainActivity (Kotlin)               │
│  ├─ starts libgophersun.so          │  ← Go backend: GOOS=android, CGO_ENABLED=0
│  └─ WebView → http://127.0.0.1:8765 │  ← same frontend as desktop
└─────────────────────────────────────┘
```

- `src/main.go` — desktop (webview), `src/main_android.go` — Android (no webview), shared core in `src/app.go`.
- `src/dns_android.go` — DNS for pure Go on Android (no `/etc/resolv.conf`).
- `src/server/sync.go`, `src/server/sync_tls.go`, `src/storage/sync.go` — sync, TLS and QR pairing.
- `android/` — Kotlin shell (WebView, camera, permissions, system bar insets).
