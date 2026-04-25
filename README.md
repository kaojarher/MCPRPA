# MCP · RPA Controller — 三種執行模式完整說明

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
