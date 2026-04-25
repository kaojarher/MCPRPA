# MCP · RPA Controller — Complete Guide to Three Execution Modes

> Version: v2.0 All-in-One &nbsp;|&nbsp; File: `mcp-rpa-all-in-one.html`

---

## Table of Contents

1. [Tool Overview](#tool-overview)
2. [Mode A: Simulate Mode](#mode-a-simulate-mode)
3. [Mode B: Extension Mode (Recommended)](#mode-b-extension-mode-recommended)
4. [Mode C: CDP WebSocket Mode](#mode-c-cdp-websocket-mode)
5. [Mode Comparison Table](#mode-comparison-table)
6. [FAQ](#faq)

---

## Tool Overview

`mcp-rpa-all-in-one.html` is a **single-file RPA design and execution tool** that includes:

- Visual script editor with drag-and-drop step ordering
- Built-in Gemini conversation script library (5 scripts)
- Script import / export in JSON format
- Three execution engines (Simulate / Extension / CDP)
- Real-time execution log panel
- Environment diagnostics tool

**Basic workflow:**

```
Open HTML → Pick a script from the library → Load into editor → Select execution mode → ▶ Run
```

---

## Mode A: Simulate Mode

### Who it's for

- First-time users who want to explore the tool without any setup
- Designing and testing script logic without touching the browser
- Anyone who does not want to install additional software or extensions

### Capabilities

| Feature | Details |
|---------|---------|
| Real browser control | ✗ Browser is not touched |
| Installation required | ✓ None — open and use |
| Screenshot download | ✗ Not supported |
| Cookie operations | ✗ Not supported |
| Best suited for | Script design, logic validation |

### Getting Started

**Step 1** — Open the tool

Double-click `mcp-rpa-all-in-one.html` to open it in Chrome.

**Step 2** — Load a script

Click the **Script Library** tab at the top of the center panel → Under the **🤖 Built-in · Gemini Series** section, click any script, for example:

```
① Open Gemini and Send a Greeting
```

The script loads automatically into the editor, showing all steps.

**Step 3** — Confirm execution mode

Look at the three mode buttons in the bottom execution bar on the right side. Confirm that **⬡ Simulate is highlighted** — this is the default.

```
⬡ Simulate  ← should be highlighted in blue
⬡ EXT
⬡ CDP
```

**Step 4** — Run the script

Click the **▶ Run** button on the bottom left and observe:

- The right-side log panel shows each step's output in sequence
- Each step card displays a **✓** in the top-right corner
- The progress bar advances to 100%

### Sample Simulate Mode Log

```
[10:23:41] INFO  ▶ Start: Open Gemini and Send a Greeting (8 steps) [simulate]
[10:23:41] INFO  [1/8] → https://gemini.google.com
[10:23:41] SYS   → Navigate https://gemini.google.com
[10:23:41] INFO  [2/8] wait[visible] .ql-editor
[10:23:42] OK    ✓ Done
...
[10:23:43] OK    ▣ Execution complete
```

> **Note:** In Simulate mode, Chrome performs no actual actions. All steps are recorded as log entries only — no real web page interaction takes place.

---

## Mode B: Extension Mode (Recommended)

### Who it's for

- Users who want scripts to **genuinely control Chrome**
- Those who prefer not to configure a debugging port
- **Best choice for beginners:** install once, then use effortlessly every time

### Capabilities

| Feature | Details |
|---------|---------|
| Real browser control | ✓ Fully real |
| Installation required | One-time Chrome Extension install |
| Debugging port required | ✗ Not needed |
| Screenshot download | ✓ Auto-downloads as PNG |
| Cookie operations | ✓ Full support |
| Cross-site support | ✓ All websites |

### Installation (One-Time Setup)

**Step 1** — Download the Extension

In the tool, switch to the **Extension Install** tab → click **⬇ Download Extension ZIP**.

Alternatively, use the `mcp-rpa-extension.zip` file you already downloaded.

**Step 2** — Unzip the file

Extract the ZIP to a **permanent location** (e.g. a dedicated folder on your Desktop).

```
Desktop/
└── mcp-rpa-extension/
    ├── manifest.json
    ├── background.js
    ├── content.js
    ├── popup.html
    └── icons/
```

> ⚠️ **Important:** Do **not** move or rename this folder after installation. If you do, the Extension will break and must be reloaded from the new location.

**Step 3** — Open Chrome's Extension management page

Type the following into the Chrome address bar:

```
chrome://extensions/
```

**Step 4** — Enable Developer Mode

Toggle the **Developer mode** switch in the **top-right corner** of the page to **On** (it turns blue).

**Step 5** — Load the Extension

Click **Load unpacked** in the top-left → select the folder you extracted in Step 2 (select the entire folder, not an individual file inside it).

**Step 6** — Confirm successful installation

A **hexagonal ⬡ icon** appears in the Chrome toolbar — installation is complete.

```
[Chrome Toolbar] ... ⬡ ...
```

### Daily Usage

**Step 1** — Open the tool and detect the Extension

Open `mcp-rpa-all-in-one.html` in Chrome → click **⬡ EXT** at the bottom → click **◎ Detect Extension**.

The following message in the log confirms a successful connection:

```
[10:25:01] OK    ✓ Extension is ready
```

**Step 2** — Load a script

Go to **Script Library** → click **① Open Gemini and Send a Greeting**.

**Step 3** — Run

Confirm **⬡ EXT** is highlighted at the bottom → click **▶ Run** → watch Chrome automate Gemini in real time.

### How Extension Mode Works

```
mcp-rpa-all-in-one.html
        │
        │  postMessage (MCP_RPA_EXEC)
        ▼
content.js  (injected into every page)
        │
        │  chrome.runtime.sendMessage
        ▼
background.js  (Service Worker)
        │
        │  chrome.scripting / chrome.tabs API
        ▼
Chrome tab  (real web page being automated)
```

### Full Command Reference

| Category | Commands |
|----------|----------|
| Navigation | `navigate`, `reload`, `back`, `forward`, `tab_new`, `tab_close` |
| Interaction | `click`, `dblclick`, `hover`, `type`, `select`, `key_press` |
| Scrolling | `scroll`, `scroll_to` |
| Waiting | `wait`, `wait_selector`, `wait_url` |
| Extraction | `extract_text`, `extract_attr`, `get_url` |
| Screenshot | `screenshot` (auto-downloads PNG) |
| JavaScript | `js_eval`, `js_inject` |
| Cookie / Storage | `cookie_get`, `cookie_set`, `storage_get` |
| Logic | `condition`, `loop` |

---

## Mode C: CDP WebSocket Mode

### Who it's for

- Advanced users who need fine-grained control via Chrome DevTools Protocol
- Developers comfortable with command-line tools
- Users who need screenshots or full browser control without installing an Extension

### Capabilities

| Feature | Details |
|---------|---------|
| Real browser control | ✓ Fully real |
| Installation required | ✗ No Extension needed |
| Debugging port required | ✓ Must pass flag every time Chrome launches |
| Screenshot download | ✓ Auto-downloads PNG |
| Cookie operations | △ Partial (requires JS workaround) |
| Cross-site support | ✓ Full support |

### Setup Steps

**Step 1** — Fully close Chrome

Make sure every Chrome window — including background processes — is closed.

```bash
# Windows (Command Prompt)
taskkill /F /IM chrome.exe /T

# macOS (Terminal)
pkill -9 "Google Chrome"
```

**Step 2** — Launch Chrome in debug mode

Run the command for your operating system:

**Windows:**

```bash
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222
```

Or press **Win + R** and type:

```
chrome.exe --remote-debugging-port=9222
```

**macOS:**

```bash
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222
```

**Linux:**

```bash
google-chrome --remote-debugging-port=9222
```

> ⚠️ **Important:** Chrome must be fully closed before running the command above. If Chrome is already running, the `--remote-debugging-port` flag is silently ignored and the debugging port will not open.

**Step 3** — Verify the debugging port is active

In the Chrome window you just launched, navigate to:

```
http://localhost:9222/json/version
```

If you see a JSON response similar to the following, you're good to go:

```json
{
  "Browser": "Chrome/124.0.0.0",
  "Protocol-Version": "1.3",
  "webSocketDebuggerUrl": "ws://localhost:9222/devtools/browser/..."
}
```

If the page fails to load, return to Step 1 and try again.

**Step 4** — Run the environment diagnostics

In the tool, switch to the **MCP Settings** tab → click **▶ Run Diagnostics**.

All items should show a green ✓ before proceeding:

```
✓  Chrome port :9222    Ready · Chrome/124.0.0.0
✓  Controllable tabs    3 found
→  Current tab          New Tab
★  Next step            Click "Connect CDP" → select CDP mode → Run
```

**Step 5** — Connect to CDP

Click **⬡ Connect CDP**. The status indicator changes to:

```
● CDP Connected
```

**Step 6** — Switch mode and run

Click **⬡ CDP** at the bottom → load a script → click **▶ Run**.

### CDP Mode Troubleshooting

**Q: "WebSocket not connected" error during execution**

Confirm Chrome was launched with the debug flag. Visit `http://localhost:9222/json/version` again to verify the port is open.

**Q: The connection drops after a while**

CDP connections close when Chrome is shut down or the user switches profiles. Relaunch Chrome in debug mode and click **⬡ Connect CDP** again to reconnect.

**Q: Chrome shows an account selection screen on startup**

This is normal when launching with a fresh profile. Simply select an account to proceed — the account selection does not affect the debugging port.

---

## Mode Comparison Table

| Feature | ⬡ Simulate | ⬡ EXT (Recommended) | ⬡ CDP |
|---------|:---------:|:-------------------:|:-----:|
| Real browser control | ✗ | ✓ | ✓ |
| Extension install required | ✗ | ✓ (one-time) | ✗ |
| Debugging port required | ✗ | ✗ | ✓ |
| Screenshot download | ✗ | ✓ | ✓ |
| Full Cookie support | ✗ | ✓ | △ |
| Cross-site automation | ✗ | ✓ | ✓ |
| Difficulty | Easiest | Easy | Intermediate |
| Best for | Script design | Daily automation | Advanced development |

---

## FAQ

### General

**Q: Can I use the HTML tool offline?**

Yes. The tool itself requires no internet connection. Google Fonts will not load offline, but this only affects the font appearance — all functionality works normally.

**Q: Are scripts saved automatically?**

The script library uses the browser's `localStorage`. It persists across sessions but is cleared if you clear browser data. Export your scripts regularly using the **↓ Export** button to keep JSON backups.

**Q: Can I use this in Edge or Firefox?**

Extension mode is only supported in Chromium-based browsers (Chrome, Edge, Brave). Simulate mode and CDP mode work in all modern browsers.

**Q: The `.ql-editor` selector for Gemini stopped working. What do I do?**

Google may update Gemini's HTML structure over time. Open Chrome DevTools (F12), inspect the input field, find its current CSS selector, and update the step card in the editor accordingly.

---

### Extension Mode

**Q: "◎ Detect Extension" always shows "not detected"**

Check the following:

1. The Extension is actually loaded in `chrome://extensions/` (look for the hexagonal ⬡ icon in the toolbar)
2. The HTML tool is opened in **the same Chrome browser** where the Extension is installed
3. Try refreshing the HTML tool page, then click **Detect Extension** again

**Q: Can I move the Extension folder after installation?**

No. Moving or renaming the folder breaks the Extension and causes an error badge on the icon. To fix it, remove the broken entry in `chrome://extensions/`, then click **Load unpacked** again pointing to the new folder path.

---

### CDP Mode

**Q: `localhost:9222` is unreachable even though Chrome is open**

The most common cause is a **lingering Chrome background process** that was already running before you launched the debug command. Use Task Manager (Windows) or Activity Monitor (macOS) to confirm all Chrome processes are terminated, then rerun the debug launch command.

**Q: Do I have to type the launch command every time?**

No — create a **desktop shortcut**:

- **Windows:** Right-click the Chrome shortcut → Properties → append ` --remote-debugging-port=9222` to the end of the **Target** field → OK. Use this shortcut to launch Chrome.
- **macOS:** Create a small shell script or use Automator to wrap the Terminal command, then save it as an application on your Desktop.

---

*MCP · RPA Controller v2.0 All-in-One*  
*Documentation version: April 2026*# MCP · RPA Controller — 三種執行模式完整說明

> 版本：v2.0 All-in-One｜適用檔案：`mcp-rpa-all-in-one.html`

---

## 目錄

1. [工具概覽](#工具概覽)
2. [模式 A：模擬模式](#模式-a模擬模式)
3. [模式 B：Extension 模式（推薦）](#模式-bextension-模式推薦)
4. [模式 C：CDP WebSocket 模式](#模式-ccdp-websocket-模式)
5. [三種模式比較表](#三種模式比較表)
6. [常見問題 FAQ](#常見問題-faq)

---

## 工具概覽

`mcp-rpa-all-in-one.html` 是一個**單檔 RPA 設計與執行工具**，整合了：

- 視覺化腳本編輯器（拖曳式）
- 內建 Gemini 對話系列腳本庫（5 個）
- 腳本匯入／匯出（JSON 格式）
- 三種執行引擎（模擬 / Extension / CDP）
- 執行日誌面板
- 環境診斷工具

**基本使用流程：**

```
開啟 HTML → 腳本庫選腳本 → 載入編輯器 → 選擇執行模式 → ▶ 執行
```

---

## 模式 A：模擬模式

### 適合對象

- 第一次使用，想了解工具操作方式
- 設計與測試腳本邏輯，不需要真實操作瀏覽器
- 不想安裝任何額外軟體或 Extension

### 特性

| 項目 | 說明 |
|------|------|
| 真實執行 | ✗ 不操作瀏覽器 |
| 安裝需求 | ✓ 無需安裝任何東西 |
| 截圖功能 | ✗ 不支援 |
| Cookie 操作 | ✗ 不支援 |
| 適合場景 | 腳本設計、邏輯驗證 |

### 啟動步驟

**Step 1** — 下載並開啟工具

雙擊 `mcp-rpa-all-in-one.html`，用 Chrome 瀏覽器開啟。

**Step 2** — 載入腳本

點擊中間上方的「**腳本庫**」頁籤 → 在「🤖 內建 · Gemini 對話系列」區塊點擊任一腳本，例如：

```
① 打開 Gemini 並問候
```

腳本會自動載入到編輯器，可看到所有步驟。

**Step 3** — 確認執行模式

查看底部執行列右側的三個按鈕，確認**「⬡ 模擬」已亮起**（預設即為此模式）。

```
⬡ 模擬  ← 應為藍色高亮
⬡ EXT
⬡ CDP
```

**Step 4** — 執行腳本

點擊底部左側的「**▶ 執行**」按鈕，觀察：

- 右側日誌面板逐步顯示各步驟記錄
- 每個步驟卡片右上角出現「✓」
- 進度條推進至 100%

### 模擬模式日誌範例

```
[10:23:41] INFO  ▶ 開始：打開 Gemini 並問候（8 步）[simulate]
[10:23:41] INFO  [1/8] → https://gemini.google.com
[10:23:41] SYS   → 導覽 https://gemini.google.com
[10:23:41] INFO  [2/8] wait[visible] .ql-editor
[10:23:42] OK    ✓ 完成
...
[10:23:43] OK    ▣ 執行完畢
```

> **注意：** 模擬模式下，Chrome 不會有任何動作。所有步驟均為記錄性質，不會真正操作網頁。

---

## 模式 B：Extension 模式（推薦）

### 適合對象

- 希望腳本**真實操作 Chrome** 的使用者
- 不想設定除錯埠的一般使用者
- **新手首選**：安裝一次，之後使用最簡便

### 特性

| 項目 | 說明 |
|------|------|
| 真實執行 | ✓ 完全真實 |
| 安裝需求 | 一次性安裝 Chrome Extension |
| 需開除錯埠 | ✗ 不需要 |
| 截圖功能 | ✓ 自動下載 PNG |
| Cookie 操作 | ✓ 完整支援 |
| 跨網站支援 | ✓ 所有網站 |

### 安裝步驟（一次性）

**Step 1** — 下載 Extension

在工具內切換到「**Extension 安裝**」頁籤 → 點擊「**⬇ 下載 Extension ZIP**」。

或直接從已下載的 `mcp-rpa-extension.zip` 開始。

**Step 2** — 解壓縮

將 ZIP 解壓縮到一個**固定位置**（例如桌面建一個資料夾）。

```
桌面/
└── mcp-rpa-extension/
    ├── manifest.json
    ├── background.js
    ├── content.js
    ├── popup.html
    └── icons/
```

> ⚠️ **重要：** 資料夾路徑之後**不能移動或刪除**，否則 Extension 會失效，需重新載入。

**Step 3** — 開啟 Chrome Extension 管理頁

在 Chrome 網址列輸入：

```
chrome://extensions/
```

**Step 4** — 開啟開發人員模式

點擊頁面**右上角**的「**開發人員模式**」開關，切換為**開啟**（變成藍色）。

**Step 5** — 載入 Extension

點擊左上角「**載入未封裝項目**」→ 選擇 Step 2 解壓縮的**資料夾**（選整個資料夾，不是裡面的單一檔案）。

**Step 6** — 確認安裝成功

Chrome 工具列出現**六角形 ⬡ 圖示**即代表安裝成功。

```
[Chrome 工具列] ... ⬡ ...
```

### 使用步驟

**Step 1** — 開啟工具並偵測 Extension

用 Chrome 開啟 `mcp-rpa-all-in-one.html` → 底部點擊「**⬡ EXT**」→ 點擊「**◎ 偵測 Extension**」。

日誌出現以下訊息代表連線成功：

```
[10:25:01] OK    ✓ Extension 已就緒
```

**Step 2** — 載入腳本

腳本庫 → 點擊「① 打開 Gemini 並問候」。

**Step 3** — 執行

確認底部「**⬡ EXT**」已亮起 → 點「**▶ 執行**」→ 觀察 Chrome 自動操作 Gemini。

### Extension 模式運作原理

```
mcp-rpa-all-in-one.html
        │
        │  postMessage (MCP_RPA_EXEC)
        ▼
content.js（注入每個頁面）
        │
        │  chrome.runtime.sendMessage
        ▼
background.js（Service Worker）
        │
        │  chrome.scripting / chrome.tabs API
        ▼
Chrome 分頁（真實網頁被操作）
```

### Extension 支援的完整指令

| 類別 | 指令 |
|------|------|
| 導覽 | `navigate`、`reload`、`back`、`forward`、`tab_new`、`tab_close` |
| 互動 | `click`、`dblclick`、`hover`、`type`、`select`、`key_press` |
| 捲動 | `scroll`、`scroll_to` |
| 等待 | `wait`、`wait_selector`、`wait_url` |
| 擷取 | `extract_text`、`extract_attr`、`get_url` |
| 截圖 | `screenshot`（自動下載 PNG） |
| JS | `js_eval`、`js_inject` |
| Cookie | `cookie_get`、`cookie_set`、`storage_get` |
| 邏輯 | `condition`、`loop` |

---

## 模式 C：CDP WebSocket 模式

### 適合對象

- 需要精細控制 Chrome DevTools Protocol 的進階使用者
- 有程式開發背景，熟悉命令列操作
- 需要截圖（PNG 直接下載）且不想安裝 Extension

### 特性

| 項目 | 說明 |
|------|------|
| 真實執行 | ✓ 完全真實 |
| 安裝需求 | ✗ 不需安裝 Extension |
| 需開除錯埠 | ✓ 每次啟動 Chrome 須帶參數 |
| 截圖功能 | ✓ 自動下載 PNG |
| Cookie 操作 | △ 部分支援（需透過 JS 擷取） |
| 跨網站支援 | ✓ 完整支援 |

### 啟動步驟

**Step 1** — 完全關閉 Chrome

確保所有 Chrome 視窗已關閉，**包含背景程序**。

```bash
# Windows（命令提示字元）
taskkill /F /IM chrome.exe /T

# macOS（終端機）
pkill -9 "Google Chrome"
```

**Step 2** — 以除錯模式啟動 Chrome

根據你的作業系統執行對應指令：

**Windows：**

```bash
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222
```

或直接在「執行」（Win+R）輸入：

```
chrome.exe --remote-debugging-port=9222
```

**macOS：**

```bash
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222
```

**Linux：**

```bash
google-chrome --remote-debugging-port=9222
```

> ⚠️ **重要：** 必須先完全關閉 Chrome 再執行上述指令。若 Chrome 已在執行中，`--remote-debugging-port` 參數會被忽略，除錯埠不會開啟。

**Step 3** — 驗證除錯埠是否成功開啟

在**剛才開啟的 Chrome 視窗**中，網址列輸入：

```
http://localhost:9222/json/version
```

若看到類似以下的 JSON 代碼，代表成功：

```json
{
  "Browser": "Chrome/124.0.0.0",
  "Protocol-Version": "1.3",
  "webSocketDebuggerUrl": "ws://localhost:9222/devtools/browser/..."
}
```

若顯示「無法連線」，請回到 Step 1 重新執行。

**Step 4** — 執行環境診斷

在工具內切換到「**MCP 設定**」頁籤 → 點擊「**▶ 執行診斷**」。

所有項目顯示綠色 ✓ 才繼續下一步：

```
✓  Chrome port :9222    就緒 · Chrome/124.0.0.0
✓  可控制分頁          3 個
→  目前分頁            新分頁
★  下一步              點擊「連線 CDP」→ 選 CDP 模式 → 執行
```

**Step 5** — 連線 CDP

點擊「**⬡ 連線 CDP**」，狀態列變成：

```
● CDP 已連線
```

**Step 6** — 切換模式並執行

底部點擊「**⬡ CDP**」→ 載入腳本 → 點「**▶ 執行**」。

### CDP 模式常見問題

**Q：執行時出現「WebSocket 未連線」**

確認 Chrome 是以除錯模式啟動的。可再次訪問 `http://localhost:9222/json/version` 驗證。

**Q：連線後瀏覽器又斷線了**

CDP 連線在 Chrome 關閉或切換使用者時會中斷。重新啟動除錯模式的 Chrome 後，點「**⬡ 連線 CDP**」重連。

**Q：Chrome 帳戶選擇畫面怎麼辦**

以除錯模式啟動後若出現帳戶選擇畫面，正常選擇帳號進入即可。帳戶選擇不影響除錯埠的運作。

---

## 三種模式比較表

| 功能 | ⬡ 模擬 | ⬡ EXT（推薦） | ⬡ CDP |
|------|:------:|:------------:|:-----:|
| 真實操作瀏覽器 | ✗ | ✓ | ✓ |
| 需安裝 Extension | ✗ | ✓（一次性） | ✗ |
| 需開除錯埠 | ✗ | ✗ | ✓ |
| 截圖下載 | ✗ | ✓ | ✓ |
| Cookie 完整支援 | ✗ | ✓ | △ |
| 跨網站操作 | ✗ | ✓ | ✓ |
| 使用難度 | 最低 | 低 | 中 |
| 適合場景 | 設計腳本 | 日常自動化 | 進階開發 |

---

## 常見問題 FAQ

### 通用

**Q：HTML 工具可以離線使用嗎？**

可以。工具本身不需要網路，但 Google Fonts 字型需要網路才能正常載入（不影響功能，只影響字型顯示）。

**Q：腳本會自動儲存嗎？**

腳本庫使用瀏覽器 `localStorage` 儲存。清除瀏覽器資料時會一併清除，建議定期用「↓ 匯出」備份 JSON 檔案。

**Q：可以在 Edge 或 Firefox 使用嗎？**

Extension 模式僅支援 Chromium 核心的瀏覽器（Chrome、Edge、Brave）。模擬模式和 CDP 模式在所有現代瀏覽器均可正常運作。

**Q：Gemini 的 `.ql-editor` 選擇器失效怎麼辦？**

Google 可能更新 Gemini 的 HTML 結構。請用 Chrome 開發者工具（F12）重新找到輸入框的選擇器，在步驟卡片中更新即可。

### Extension 模式

**Q：「◎ 偵測 Extension」一直顯示未偵測**

請確認：
1. Extension 確實已在 `chrome://extensions/` 中載入（可看到六角形圖示）
2. HTML 工具是用**安裝了 Extension 的同一個 Chrome** 開啟的
3. 嘗試重新整理 HTML 工具頁面後再偵測

**Q：Extension 安裝後可以移動資料夾嗎？**

不行。移動或重命名資料夾後，Extension 會失效並顯示錯誤。請在 `chrome://extensions/` 中移除舊的，再重新載入新路徑的資料夾。

### CDP 模式

**Q：`localhost:9222` 連不上，但 Chrome 確實有開啟**

最常見原因是 Chrome 已有**既有的背景程序**在執行。請用工作管理員（Windows）或活動監視器（macOS）確認所有 Chrome 程序都已結束，再重新執行除錯指令。

**Q：每次都要重新輸入啟動指令，有沒有更方便的方法？**

可以建立一個**桌面捷徑**：右鍵 Chrome 圖示 → 內容 → 在「目標」欄位末端加上 ` --remote-debugging-port=9222`。之後透過此捷徑啟動 Chrome 就會自動開啟除錯埠。

---

*MCP · RPA Controller v2.0 All-in-One*
*文件版本：2026-04*
