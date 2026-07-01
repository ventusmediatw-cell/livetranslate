# LiveTranslate（即時語音翻譯）

完全在瀏覽器裡跑的**即時語音翻譯**，專為面對面對話設計。你講話，對方就即時讀到他語言的字幕——可選同時播出翻譯語音。底層用 Google 的 Gemini Live Translate API。

免安裝、免註冊、無後端。你自備一把 Google Gemini API key（只存在你的瀏覽器裡），就這樣。

**支援語言：** 高棉文 · 中文 · 英文 · 越南文 · 泰文 · 日文 · 韓文（以及模型支援的其他語言）。

[English →](README.md)

---

## 特色

- 🎙️ **即時串流翻譯** — 開始講話約 3–4 秒後字幕出現，之後一句句跟上。
- 🔊 **字幕 + 可選語音** — 顯示翻譯文字，也可把翻譯語音直接播出來。
- 🌐 **自動語言偵測** — 自動判斷講者的語言。
- 💻📱 **純瀏覽器 App** — 桌面、手機瀏覽器都能跑，可加到主畫面。
- 🔑 **自備 key** — 你的 Gemini API key 只存在瀏覽器（`localStorage`）、只送給 Google，不經過任何其他伺服器。
- 🆓 **無後端、$0 架設** — 就是一個靜態 HTML 檔，丟哪都行，或本機直接開。

## 快速開始

1. **取得 Google Gemini API key**（免費，見下）。
2. **打開 App**（見「怎麼跑」）。
3. 點右上 **⚙ 齒輪**，貼上你的 key。
4. 選要**譯成**的語言。
5. 按 **● 按鈕**開始講話，跳出麥克風權限時允許。

## 取得 Gemini API key

1. 前往 **[aistudio.google.com/apikey](https://aistudio.google.com/apikey)**。
2. 用 Google 帳號登入。
3. 點 **Create API key**，複製下來（長得像 `AIza…`）。
4. 貼進 App 設定裡。完成——它只存在你的瀏覽器。

> **關於方案與隱私。** 免費層就能用——免費 Gemini key（不綁卡、不用信用卡）跑得動即時模型。免費層會**限流**（重度使用可能撞 `429` 配額），且免費層**你的音訊會被 Google 拿去改進產品、也可能被真人審閱**——機密對話請勿用免費 key。要更高配額 + 隱私，就開啟計費（付費層），付費層不會拿你的內容做訓練。

## 怎麼跑

App 需要**安全環境**（HTTPS 或 `localhost`）才能取用麥克風。三選一：

- **GitHub Pages（最簡單）：** fork 這個 repo → *Settings → Pages → 從 `main` 部署*。App 就上線在 `https://<你的帳號>.github.io/livetranslate/`。
- **任何靜態託管：** Netlify、Vercel、Cloudflare Pages、Firebase Hosting——放上 `index.html` 即可。
- **本機：** 在 repo 資料夾跑靜態 server，開 localhost：
  ```bash
  python3 -m http.server 8000
  # 然後開 http://localhost:8000
  ```
  （直接用 `file://` 開 `index.html` **不行**，瀏覽器會擋麥克風。）

## 隱私

- **你的 API key** 只存在瀏覽器 `localStorage`，只送到 Google 的 API 端點，不會給任何第三方。
- **你的音訊**會送到 Google 進行翻譯。在**免費層**，Google 可能拿你送出的音訊來改進其產品、也可能被真人審閱。**機密對話請勿用免費層 key。** 要避免，請開啟計費（付費層），付費層不會拿你的內容做訓練。

## 運作原理

瀏覽器擷取麥克風音訊（Web Audio API、16 kHz PCM），直接串流到 Gemini Live Translate WebSocket，把回傳的轉寫 + 翻譯顯示成字幕（並可播放 24 kHz 譯文語音）。中間沒有伺服器——網頁直接拿你的 key 跟 Google 對話。

## 限制與誠實說明

- **無法鎖定來源語言。** Live Translate API 自動偵測講者語言、沒有覆寫選項。相似語言（例如**高棉文 vs. 越南文**）可能被誤判，進而讓翻譯出錯。若發生，停止後重新開始重新偵測。（未來的「鎖定」模式會用另一個 Gemini 模型強制來源語言——見 Roadmap。）
- **手機僅前景。** 手機瀏覽器在分頁不可見時會關掉麥克風（平台規則），所以請保持螢幕亮、App 在前景。瀏覽器做不到背景/鎖屏翻譯。
- **預覽模型。** 依賴的是預覽模型，其可用性與方案可能變動。

## Roadmap

- 鎖定來源模式（分段、強制講者語言——解掉高棉/越南誤判）。
- 手機打磨：Screen Wake Lock、PWA 安裝 manifest、iOS 音訊調校。

## 授權

MIT — 見 [LICENSE](LICENSE)。
