---
type: lab
tags:
  - teaching
  - msba6125
  - lab
  - guide
created: 2026-09-21
updated: 2026-09-21
---

# 第 5 課｜Hermes Agent 安裝與首次商務任務

> 本實作為 Hermes 平台線第一課，由兩個單元組成。單元一在你的電腦安裝本地 AI 助手 Hermes Desktop，以 Nous Portal 免費方案連接模型，並完成首次對話與文件權限測試——**建議課前在家完成**（安裝需下載數百 MB，在家完成可讓課堂時間用於任務與討論）。單元二為課堂任務：以助手完成兩項日常商務任務，練習「在同一對話追加指示」的迭代方式，最後進行小組討論。

## 學習目標（Learning Objectives）

完成本實作後，你將具備：

- 在自己的電腦安裝 Hermes Desktop，並以免費方案（Nous Portal Free plan）連接免費模型
- 說明 agentic AI 助手與單純聊天機器人的差異：工具、記憶與自主執行
- 以文件存取授權為實例，說明本地 AI 助手的權限管理觀念
- 與助手協作完成郵件、會議與行程三類商務任務中的兩項
- 輸出不理想時，在既有對話中追加指示改良輸出，而非另開對話重新開始

## 課前準備（Pre-class Requirement）

1. 電腦：Windows 10／11（本手冊以 Windows 為基準）；磁碟可用空間不少於 10 GB
2. 可用的 Google 或 GitHub 帳號（可完成瀏覽器登入）
3. **建議先在家中完成單元一**：安裝需下載數百 MB 至 1 GB 級檔案，視網絡約 5–15 分鐘。若未及在家完成：課堂上即時安裝（約 10–20 分鐘）；時間不足時可先與同組已完成安裝者共用一台機器，並於課後在家補完
4. 若使用 VPN 或代理且下載、連線緩慢：先關閉後重試（見下方「常見問題」）

## 單元一：安裝 Hermes Desktop 與連接免費模型（建議課前在家完成）

### 1.1 概念：什麼是 Agentic AI

AI 助手（agent）與單純聊天機器人的差別在於三件事：**工具**（可以讀寫文件、搜索網頁、查詢數據庫）、**記憶**（跨對話保留你的設定與偏好）與**自主執行**（依你的指示完成多步驟任務，而非只回一段文字）。Hermes Agent 是一套安裝於你電腦本地的 AI 助手框架，由 Nous Research 開發：你的對話資料與設定存放於本機，模型連線則經由你選擇的雲端服務（本課使用免費的 Nous Portal 方案）。

官方概念與快速入門見 Hermes Agent 官方文件：官方入口 https://hermes-agent.nousresearch.com/docs/getting-started/quickstart 。

### 1.2 下載與安裝

1. 瀏覽器開啓官方下載頁：官方入口 https://hermes-agent.nousresearch.com/desktop
2. 按 **Windows** 下載按鈕，取得安裝檔 `Hermes-Setup.exe`（檔案由 Nous Research Inc. 簽名）。
3. 於下載資料夾雙擊執行。**預期畫面**：
   - 若出現 Windows SmartScreen「Windows 已保護您的電腦」：按 **更多信息** → **仍要運行**（首次運行新軟件屬正常防護，安裝檔有簽名）
   - 主畫面按 **INSTALL** 開始安裝
   - 畫面顯示安裝進度（自動安裝 Python、Node.js、Git 與 Hermes 核心，並建立虛擬環境）——期間不要關閉窗口或讓電腦休眠
   - 安裝失敗時記下錯誤文字，見下方「常見問題」之「安裝失敗」條目處理
4. **安裝完成後先不要按 LAUNCH**：先依 1.3 節把 Hermes 資料夾移至課程統一位置（首次啓動前搬移，可避免檔案佔用問題），再回來啓動。
5. （可選）安裝後檢查：開始功能表搜尋 **PowerShell** → 開啓 Windows PowerShell → 執行 `hermes --version`。**預期輸出**：一行版本號（如 `hermes 0.21.0`）。若顯示「無法將 hermes 識別爲 cmdlet」：關閉全部 PowerShell 後開新窗口重試；仍失敗表示安裝器未把 hermes 加入 PATH——Desktop 使用不受影響，可跳過此檢查。

### 1.3 搬移至課程統一位置

後續操作一律以 `Documents\hermes-sandbox\hermes` 為 Hermes 資料位置。搬移以「移動＋連結」方式進行：資料實際存放於新位置，安裝器原位置保留為連結——程式運作、PATH 設定、登入狀態均不受影響。

1. 確認 Hermes 未在執行（尚未按 LAUNCH 者無此問題）。
2. 開啓 **Windows PowerShell**（開始功能表搜尋 PowerShell，不需要管理員模式），依序貼上執行以下三行（每行按 Enter 完成後再貼下一行）：

```powershell
New-Item -ItemType Directory -Force "$HOME\Documents\hermes-sandbox"
Move-Item "$env:LOCALAPPDATA\hermes" "$HOME\Documents\hermes-sandbox\hermes"
New-Item -ItemType Junction -Path "$env:LOCALAPPDATA\hermes" -Target "$HOME\Documents\hermes-sandbox\hermes"
```

**預期**：三行均無錯誤訊息。搬移後 Hermes 資料實際存放於 `C:\Users\<你的用戶名>\Documents\hermes-sandbox\hermes`（「資料與工具速查」以此位置為準）；安裝器原位置 `%LOCALAPPDATA%\hermes` 仍指向同一份資料。若第一行出現「已存在」提示或第三行報「已存在」錯誤：表示先前已搬移過，可直接跳到下一步。

3. 回到安裝器視窗按 **LAUNCH** 啓動（安裝視窗已關閉者：由開始功能表或桌面捷徑開啓 Hermes）。

遇到搬移錯誤（檔案被佔用或拒絕存取）：關閉所有 Hermes 視窗，並於系統匣（工作列右下角）的 Hermes 圖示按右鍵選「結束」，再重跑該三行指令。

### 1.4 連接免費模型（Nous Portal）

1. 按 **LAUNCH**（或桌面捷徑）啓動 Hermes Desktop。
2. **預期畫面**：出現設定畫面 "Let's get you set up"——選擇 **Nous Portal**（標示 Recommended）。
3. 瀏覽器自動開啓 Nous Portal 登入頁（官方入口 https://portal.nousresearch.com ）：
   - 沒有帳號者按 **Sign up**；已有帳號者直接登錄。登錄方式以當日頁面提供者爲準，**Google 或 GitHub 皆可**
   - 選擇 **Free plan**
   - 按 **SUBSCRIBE AND CONNECT**。若頁面出現 Stripe 信用卡輸入欄：免費方案不需要信用卡——捲到方案列表下方找 **CONNECT** 按鈕
   - 頁面顯示 "CONNECTED"（或授權成功信息）後可關閉瀏覽器
4. 回到 Hermes Desktop（會自動接續）：「Default model」卡片按 **Change** → 搜索框輸入 **free** → 選擇名稱以 `:free`／`:Free` 結尾的模型——免費模型名單會輪換，以當日顯示為準；**課堂上請各自選擇不同的 `:free` 模型**（分散負載，避免全班集中使用同一模型）。
5. 若選定後 Hermes 對該模型顯示「並非為工具使用設計」類警示：改選其他 `:free` 模型。
6. 按 **Start chatting** 進入對話——前往 1.5 節。

### 1.5 首次對話與介面認識

1. 在輸入框輸入以下文字並送出：`你好，請介紹你自己，並說明你目前使用的模型與 provider。`
   **預期**：助手正常回覆，且回覆中提到當前模型名稱。視窗右下角狀態列亦顯示模型名稱，可與回覆對照。
2. 認識界面：窗口左側邊欄可見 agents／skills／memory 等分區——這些是助手的工具與記憶存放區；本單元不需要改動，先觀察即可。

### 1.6 文件存取權限測試

在輸入框輸入：`請在我的「文檔」（Documents）資料夾建立一個名爲 hermes-test 的資料夾，並在裏面寫一個 test.txt 檔案，內容寫「hello from hermes」。`

**預期**：助手會請求文件寫入權限——**權限請求出現時先檢視範圍，並決定是否批准**（本地 AI 助手可存取本機檔案，權限應以最小範圍為原則；本任務授予 Documents 範圍即可）。完成後，於檔案總管開啓 `C:\Users\<你的用戶名>\Documents\hermes-test\test.txt`，應看到檔案存在且內容正確。

 **記錄**：權限請求視窗顯示的範圍，以及批准或縮小範圍之決定。

若助手回覆沒有權限機制或直接拒絕：改以更小範圍再試一次，如 `請在桌面上建立一個 test.txt，寫入「hello」`。

### 1.7 單元一檢查清單

- [ ] 安裝完成並通過首次對話（回覆與狀態列可見模型名稱）
- [ ] Hermes 資料位於 `Documents\hermes-sandbox\hermes`（可於檔案總管確認）
- [ ] 文件存取權限請求出現並完成處理——記錄請求範圍與處理方式

### 1.8 安全與用量注意

- 免費方案不等於無限：有限速機制，回應慢或暫時拒絕屬正常——稍候重試，或按視窗右下角模型名稱更換其他 `:free` 模型
- 你的對話會送往雲端模型：只使用虛構與公開資料；不要輸入個人密碼、證件號碼或任何敏感資料
- API key 與登錄狀態存放於本機（`Documents\hermes-sandbox\hermes`）：共用電腦使用後登出為宜；不要把 `config.yaml` 或 `.env` 傳給他人
- 本階段**不連接** WhatsApp／Telegram 等個人訊息平台——個人訊息帳號的 gateway 串接屬後續主題，避免課堂示範誤傳真實訊息

## 單元二：課堂任務——日常商務應用

### 2.1 開場確認

1. 開啓 Hermes Desktop；未完成單元一者：先依 1.2–1.4 節完成安裝與連接（課堂安裝約 10–20 分鐘）。
2. 確認視窗右下角狀態列顯示你所選的 `:free` 模型名稱；不確定時輸入 `你好，請說明你目前使用的模型。` 核對。

### 2.2 商務任務（三項選兩項）

以下任務全部使用**虛構資料**：免費模型多為雲端路由，你輸入的內容會送往第三方雲端模型，因此不要使用任何真實客戶、同事或公司資料。完成每個任務後，若輸出不滿意，**在同一個對話中追加指示改良輸出**（例如「把第二段改得更簡短」「加一列狀態欄」），不要另開新對話重新開始。

**任務 A：客戶郵件草稿**

輸入：`我是一家小型咖啡豆供應商的客服。請以簡體中文草擬一封回覆郵件，對象是投訴「上週訂單延遲三天、且缺少一包 500g 耶加雪菲」的客戶王先生。回覆需包含：道歉、原因說明（物流延誤，虛構）、補寄方案、下次訂單 9 折補償。語氣專業誠懇，不超過 200 字。`

**任務 B：會議紀要整理**

輸入：`以下是虛構的會議零散記錄，請整理成會議紀要，包含：決議、行動事項（負責人＋期限＋狀態欄）與未決問題。記錄：「Amy 說市場部下月預算砍 15%；Ben 提出改投短視頻渠道；大家同意先做兩週小規模測試；Carol 負責出測試方案，週五前；預算數字還要和財務對；下週一例會複審」。`

**任務 C：一週行程安排**

輸入：`我是虛構的銷售經理。請爲我安排下週一到週五的行程：每天上午留 1 小時處理郵件；週二和週四下午各有 2 小時客戶拜訪（客戶名虛構）；週三上午開季度會議；週四上午要做本月業績簡報的準備。請輸出爲表格：日期、上午、下午、備註。若發現衝突請指出並給出調整建議。`

### 2.3 檢查與迭代

完成兩項任務後，按以下要點自核並改良：

1. 核對輸出：要求是否逐項滿足（如任務 B 的欄位是否齊全）？有無捏造內容（虛構情境之外不應出現的數字或名稱）？語氣是否合適？
2. 選一份輸出，**在同一對話追加至少一輪指示改良**。示例：`請把行動事項表加上優先級一欄，並把語氣改得更正式。`
3. 記錄最有效的一項改良指示。

### 2.4 單元二檢查清單

- [ ] 完成兩項商務任務，並各檢查輸出品質
- [ ] 至少完成一輪「追加指示」改良輸出
- [ ] 記錄一項最有效的改良指示

## 小組討論題

1. 回顧今日操作：助手在哪些環節展現了「工具使用」「記憶」與「自主執行」？與你用過的網頁版聊天工具相比，體驗有何不同？
2. 本地 AI 助手（Hermes）與網頁版聊天工具各適合什麼任務？請各舉一例。（提示：檔案與系統存取、多步任務 vs 即時問答、跨裝置協作）
3. 「在同一對話追加指示」與「重開對話重做」相比，時間與結果有何差異？以今日實測經驗說明；這種工作習慣可如何應用到與同事的協作？
4. 助手請求文件權限時你如何判斷批准範圍？若企業為員工大量部署這類助手，應設哪些使用規則？（提示：最小權限、共用電腦登出、敏感資料界線）

## 常見問題

| 症狀 | 處理 |
|---|---|
| 安裝卡在 Python 階段或報版本錯誤 | 官方曾有此問題（安裝器與程式庫的 Python 版本鎖定不一致，已修正）——重新下載最新安裝檔再運行；仍失敗改用手機熱點網絡重試 |
| 下載官方網域或 npm 套件超時 | 設代理環境變量後重跑；校園網擋海外則用手機熱點 |
| 登錄頁沒有 Google 選項 | 用 GitHub 登錄（兩者皆可） |
| 見到信用卡輸入畫面 | 免費方案不需要信用卡——捲到方案列表下方找 **CONNECT** 按鈕 |
| 首次啓動模型清單空白 | 回 Nous Portal 方案頁確認已顯示 "CONNECTED"（按 CONNECT）；未解決則重開 Hermes |
| `:free` 模型回應慢或 429 | 免費層限速屬正常——稍候片刻重試，或更換其他 `:free` 模型；課堂上各自使用不同模型可分散負載 |
| 選定模型後出現「並非為工具使用設計」類警示 | 該模型未針對工具調用（tool calling）設計——改選其他 `:free` 模型（優先 Nous／Hermes 系列） |
| 1.3 節搬移報錯（檔案被佔用或拒絕存取） | Hermes 正在執行——關閉所有 Hermes 視窗，並於系統匣（工作列右下角）的 Hermes 圖示按右鍵選「結束」；再重跑該三行指令 |
| 搬移後捷徑或 `hermes` 指令失效 | 表示原位置連結未建立成功——重跑 1.3 節第三行指令（`New-Item -ItemType Junction ...`）後重試 |
| 以指令啓動 Desktop 時首次建構失敗（畫面大量 npm error） | 屬備援路線（以 PowerShell 安裝者）之情況，路線 A 不會遇到——① 錯誤含 `assert-root-install`：於程式庫根目錄執行 `npm ci` 後重跑（2026-09-05 教師機實測修法）② 錯誤含 `Access is denied` on `Hermes.exe`：關閉已開啓的 Hermes 視窗後重跑 |
| npm 指令報 log 寫入失敗（指向已不存在的磁碟） | 機器 npm cache 設定指向失效路徑——執行 `npm config set cache "C:\Users\<你的用戶名>\AppData\Local\npm-cache"` 後重跑（罕見，多見於曾改動磁碟配置的舊機器） |
| 終端機持續出現 simple-git "Invalid value for custom binary"／DEP0180 警告 | 全屬無害噪音（上游已知問題 [GitHub issue #79245](https://github.com/NousResearch/hermes-agent/issues/79245)：Windows 路徑觸發；功能不受影響），忽略即可 |

## 資料與工具速查

| 用途 | 位置 |
|---|---|
| Hermes Desktop 下載與官方文件 | https://hermes-agent.nousresearch.com/desktop ； https://hermes-agent.nousresearch.com/docs |
| Nous Portal（免費方案） | https://portal.nousresearch.com |
| Hermes 本機資料 | `C:\Users\<你的用戶名>\Documents\hermes-sandbox\hermes`（設定檔 `config.yaml`；安裝器原位置 `%LOCALAPPDATA%\hermes` 以連結指向同一資料，仍有效） |
| 工具無法使用時 | 查 AI 工具切換對照指南（已發佈於課程 GitHub 倉庫 lab/ai-tools-guide.md；Gemini↔Copilot↔WPS AI／DeepSeek 思考模式／harness 說明） |
