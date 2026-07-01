# LiveTranslate

Real-time, face-to-face **voice translation** that runs entirely in your browser. Speak, and the other person reads a live subtitle in their language — with optional spoken translation. Built on Google's Gemini Live Translate API.

No install, no account, no backend. You bring your own Google Gemini API key (it stays in your browser), and that's it.

**Languages:** Khmer · Chinese · English · Vietnamese · Thai · Japanese · Korean (and more the model supports).

[繁體中文說明 →](README.zh-Hant.md)

---

## Features

- 🎙️ **Live streaming translation** — subtitles appear ~3–4 s after you start speaking, then keep up sentence by sentence.
- 🔊 **Subtitles + optional voice** — show translated text, and optionally play the translated speech aloud.
- 🌐 **Auto language detection** — the speaker's language is detected automatically.
- 💻📱 **Pure browser app** — works on desktop and mobile browsers. Add it to your home screen.
- 🔑 **Bring your own key** — your Gemini API key is stored only in your browser (`localStorage`) and sent only to Google. Nothing goes through any other server.
- 🆓 **No backend, $0 to host** — it's a single static HTML file. Deploy it anywhere, or just open it locally.

## Quick start

1. **Get a Google Gemini API key** (free — see below).
2. **Open the app** (see *Running it*).
3. Click the **⚙ gear** (top right) and paste your key.
4. Pick the language to **translate into**.
5. Press the **● button** and start speaking. Allow microphone access when prompted.

## Getting a Gemini API key

1. Go to **[aistudio.google.com/apikey](https://aistudio.google.com/apikey)**.
2. Sign in with a Google account.
3. Click **Create API key** and copy it (it looks like `AIza…`).
4. Paste it into the app's settings. Done — it's saved in your browser only.

> **Note on billing / tiers.** This app uses Gemini's real-time model `gemini-3.5-live-translate-preview`. A free key may work, but this preview model can require **billing enabled** on your Google Cloud project. If the app gets stuck on "connecting" or won't translate, enable billing for your project in Google Cloud and try again.

## Running it

The app needs a **secure context** (HTTPS or `localhost`) to access the microphone. Pick one:

- **GitHub Pages (easiest):** fork this repo → *Settings → Pages → Deploy from `main`*. Your app goes live at `https://<your-user>.github.io/livetranslate/`.
- **Any static host:** Netlify, Vercel, Cloudflare Pages, Firebase Hosting — drop in `index.html`.
- **Locally:** run a static server from the repo folder and open the localhost URL:
  ```bash
  python3 -m http.server 8000
  # then open http://localhost:8000
  ```
  (Opening `index.html` directly as a `file://` URL will **not** work — browsers block the mic there.)

## Privacy

- **Your API key** lives only in your browser's `localStorage`. It is sent only to Google's API endpoint, never to any third party.
- **Your audio** is sent to Google to perform the translation. On the **free tier**, Google may use submitted audio to improve its products, and it may be reviewed by humans. **Do not use a free-tier key for confidential conversations.** To opt out of this, enable billing (paid tier), where prompts and responses are not used for training.

## How it works

The browser captures microphone audio (16 kHz PCM via the Web Audio API), streams it directly to the Gemini Live Translate WebSocket, and renders the returned transcription + translation as subtitles (and optional 24 kHz audio playback). There is no server in between — the page talks straight to Google with your key.

## Limitations & honest notes

- **Source language can't be locked.** The Live Translate API auto-detects the speaker's language and has no override. Similar languages (e.g. **Khmer vs. Vietnamese**) can be misdetected, which can garble the translation. If that happens, stop and restart to re-detect. (A future "locked" mode using a different Gemini model can force the source language — see the roadmap.)
- **Foreground only on mobile.** Mobile browsers mute the microphone when the tab isn't visible (a platform rule), so keep the screen on and the app in front. Background/screen-off translation isn't possible in a browser.
- **Preview model.** It depends on a preview model whose availability and tiers may change.

## Roadmap

- Locked-source mode (segmented, forces the speaker language — fixes Khmer/Vietnamese misdetection).
- Mobile polish: Screen Wake Lock, PWA install manifest, iOS audio tuning.

## License

MIT — see [LICENSE](LICENSE).
