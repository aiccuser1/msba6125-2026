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

# 第 6 課｜管理者自動儀表板：以 Hermes 連接銷售資料

> 本實作由兩個核心單元與兩個延伸單元組成，情境為虛構咖啡連鎖「半島咖啡」（Peninsula Coffee，共三間門市）：你的角色為分析人員，為門市管理者建置每週檢視的銷售儀表板。單元一為資料理解與指標定義：以 Hermes 讀取銷售資料（Excel 工作簿），輸出欄位結構、資料規模與資料品質觀察，並與助手共同定義儀表板的關鍵指標（KPI）。單元二為儀表板建置與人工核對：由助手生成 Python（Streamlit）儀表板程式並於本機啟動，在瀏覽器檢視、迭代改良，最後以 Excel 樞紐分析完成核實。延伸單元（選做，建議在家完成）：其一為將資料匯入 SQLite 資料庫，體會檔案與資料庫的差異；其二為以 MCP 連接器（excel-mcp-server）擴充助手直接操作 Excel 的能力。每個單元結束均有檢查清單，以供核對完成度。

## 學習目標（Learning Objectives）

完成本實作後，你將具備：

- 以 AI 助手讀取多工作表 Excel 資料，說明其欄位結構、資料規模與資料品質問題
- 定義管理者儀表板之關鍵指標，說明各指標的計算方式與管理意義
- 由助手生成 Streamlit 儀表板程式，於本機啟動並在瀏覽器檢視
- 以既有對話追加指示之方式迭代改良儀表板輸出
- 以 Excel 樞紐分析對照儀表板數字，完成人工核實
- 說明檔案（Excel）與資料庫（SQLite）在資料存取上的差異
- （延伸）以 MCP 連接器擴充助手對 Excel 檔案之操作能力

## 課前準備（Pre-class Requirement）

1. 完成第 5 課實作：Hermes Desktop 已安裝，並以 Nous Portal 免費方案連接模型；本實作沿用安裝位置 `C:\Users\<你的用戶名>\Documents\hermes-sandbox`
2. 確認 Hermes 使用之模型名稱以 `:free` 結尾；若出現「並非為工具使用設計」之提示，於模型設定改選其他 `:free` 模型
3. 從課程頁（Canvas 檔案區）下載資料檔 `sales_2026.xlsx`（放置位置見 1.1 節）
4. 資料為課程生成之虛構零售資料，不含任何真實客戶或公司資訊

## 單元一：資料理解與指標定義

### 1.1 建立工作資料夾

1. 開啓檔案總管（工作列上的資料夾圖示，或按鍵盤 Windows 鍵＋E）
2. 於上方位址列貼上 `C:\Users\<你的用戶名>\Documents\hermes-sandbox` 後按 Enter——此為第 5 課建立的 Hermes 資料位置
3. 於資料夾空白處按右鍵 → 新增 → 資料夾；輸入名稱 `lab-ch06` 後按 Enter 確認
4. 將已下載的 `sales_2026.xlsx` 移入 `C:\Users\<你的用戶名>\Documents\hermes-sandbox\lab-ch06\`

**預期**：資料夾 `lab-ch06` 內有 `sales_2026.xlsx` 一個檔案。

### 1.2 資料理解：請助手讀取資料

資料分析的第一步是確認資料的欄位、規模與品質——資料理解先於指標定義與圖表設計。

1. 開啓 Hermes Desktop；在對話框輸入以下文字並送出（路徑中 `<你的用戶名>` 換成你的 Windows 用戶名）：

```
請讀取 C:\Users\<你的用戶名>\Documents\hermes-sandbox\lab-ch06\sales_2026.xlsx，回報：
一、各工作表的名稱與用途
二、每張工作表的欄位、資料列數與時間範圍
三、你觀察到的資料品質問題（缺值、重複、極端值、命名不一致等，逐項列出）
請只讀取，不要修改或清理檔案。
```

**預期**：助手以交易表（orders）為主體回報——列出其欄位（訂單編號、日期、門市、商品、數量、單價、成本、營收、付款方式等）、規模（九百餘列）與時間範圍（2026 年 1 月至 9 月），並列出至少一項資料品質觀察。

2. 若助手未提及任何品質問題，追問：

```
請再逐項檢查：訂單列是否完全無重複、門市名稱是否完全一致、數量與單價是否有不合理數值、必填欄位是否有空白。逐項回答檢查結果。
```

**預期**：助手逐項回答檢查結果。將發現的品質問題抄錄下來。

### 1.3 指標定義：與助手共同設計儀表板

管理儀表板呈現什麼，取決於管理者每週要回答什麼問題。指標定義先於圖表設計。

1. 在同一對話輸入：

```
你是協助零售連鎖管理者的資料分析師。根據剛才的資料，請設計管理者每週檢視的儀表板指標清單，涵蓋：
一、總銷售額、訂單數、平均客單價
二、月度銷售趨勢
三、熱銷商品與類別營收分佈
四、各門市月度目標達成率（對照 targets 工作表）
每個指標請說明：計算方式、建議圖表類型、管理意義（管理者用來回答什麼問題）。
最後補充一至兩個你認為值得加入的指標，並說明理由。
```

2. 閱讀回覆後，記錄 KPI 清單。可請助手將清單整理為表格：`請把上述指標整理成表格：指標、計算方式、圖表類型、管理意義。`

**預期**：產出一份 KPI 清單，每項含計算方式與管理意義。

### 1.4 單元一檢查清單

- [ ] 助手完成資料理解回報（工作表清單、orders 欄位與規模、時間範圍）
- [ ] 已抄錄至少一項資料品質問題
- [ ] 已定義儀表板 KPI 清單（含計算方式與管理意義）

## 單元二：儀表板建置、啟動與人工核對

### 2.1 概念：管理者儀表板與 Streamlit

管理者儀表板將關鍵指標集中於單一畫面，取代每週手工整併的靜態報表。靜態報表每次更新均需手工重做；儀表板由程式讀取資料檔繪製畫面——此處「自動」的含義是：程式每次啟動或重新整理時重新讀取資料檔，資料檔更新後，畫面即呈現最新數字，無需重新製作報表。

儀表板需以可於瀏覽器檢視的互動程式實作；本實作採用 Streamlit——一套以 Python 撰寫資料應用的開源框架：單一程式檔即可產生含篩選器、圖表與表格的網頁介面，於本機瀏覽器執行，毋須部署伺服器。單一程式檔之形式亦便於由助手生成與迭代修改。

- Streamlit 官方文件（元件與 API 參考）：官方入口 https://docs.streamlit.io
- 官方安裝指引：官方入口 https://docs.streamlit.io/get-started/installation

### 2.2 生成儀表板程式

在同一對話輸入（延續單元一之對話；`<你的用戶名>` 換成你的用戶名）：

```
請用 Python 與 Streamlit 撰寫儀表板程式，存為 C:\Users\<你的用戶名>\Documents\hermes-sandbox\lab-ch06\dashboard.py，需求：
一、讀取同資料夾的 sales_2026.xlsx（orders 與 targets 工作表）
二、畫面上方顯示三個指標：總銷售額、訂單數、平均客單價
三、月度銷售趨勢折線圖
四、熱銷商品前 10 名與類別營收分佈圖
五、各門市月度目標達成率表格
六、側欄提供門市與日期範圍篩選器
完成後回報：程式檔案路徑、讀取的資料欄位、以及你對資料的任何處理決定。
若執行環境缺少套件（如 pandas、openpyxl），請先安裝後繼續。
```

**預期**：助手回報已建立 `dashboard.py`。於檔案總管開啓 `lab-ch06` 資料夾，可見 `dashboard.py` 與 `sales_2026.xlsx` 兩個檔案。

### 2.3 啟動儀表板

以 PowerShell 啟動：

1. 開啓 Windows PowerShell（開始功能表搜尋 PowerShell；不需要管理員模式）
2. 貼上以下第一行並按 Enter；等候安裝完成，再貼第二行並按 Enter：

```powershell
& "$HOME\Documents\hermes-sandbox\hermes\bin\uv.exe" tool install streamlit --with openpyxl
& "$HOME\.local\bin\streamlit.exe" run "$HOME\Documents\hermes-sandbox\lab-ch06\dashboard.py"
```

**預期**：第一行顯示安裝過程，最後出現 `Installed 1 executable: streamlit`；第二行顯示 `Local URL: http://localhost:8501`，瀏覽器自動開啓儀表板分頁。

- 瀏覽器未自動開啓時：手動開啓瀏覽器，於位址列輸入 `http://localhost:8501`
- 此 PowerShell 視窗須保持開啓——關閉視窗即停止儀表板

3. （替代方式）由助手啟動——在同一對話輸入：

```
請以背景方式啟動剛生成的儀表板（不要讓命令長時間佔住執行），完成後告訴我在哪個網址檢視。
```

**預期**：助手回報檢視網址（通常為 http://localhost:8501）。若助手持續執行而沒有回覆：直接於瀏覽器開啓 `http://localhost:8501` 檢視；仍無畫面則改用上述 PowerShell 方式。

### 2.4 迭代改良

在 Hermes 同一對話追加指示（以下為示例，可依你的 KPI 清單調整）：

```
儀表板可以正常檢視。請追加改良：
一、頁面標題改為「半島咖啡 門市週報儀表板」
二、月度趨勢圖加上每月數字標籤
三、類別營收分佈改為圓餅圖
完成後說明你修改了哪些部分。
```

**預期**：助手回報修改完成。回到瀏覽器分頁——畫面提示 `Source file changed` 時點選 **Rerun**（或 **Always rerun**）；或直接重新整理分頁，畫面即呈現改良後版本。

若改良後畫面報錯：將錯誤訊息整段貼回對話，請助手修正（在同一對話繼續，不要另開新對話）。

### 2.5 人工核對：Excel 樞紐分析

AI 產出的數字須人工抽查核實。以 Excel 樞紐分析獨立計算同一指標，與儀表板對照。

1. 以 Excel 開啓 `sales_2026.xlsx`，切到 `orders` 工作表
2. 點選資料區任一儲存格 → 功能區「插入」→「樞紐分析表」→ 於對話框按「確定」（預設置於新工作表）
3. 於樞紐分析表欄位窗格：將 `store` 拖至「列」區域；將 `revenue` 拖至「值」區域（確認顯示「加總」）
4. 得到各門市銷售額後，與儀表板的數字並排比較

**預期**：樞紐數字與儀表板一致。若助手在程式中對資料做了額外處理（如排除特定資料列），可能出現差異——記錄差異，並回到對話追問助手原因。

5. 抄錄核對結果：一致／不一致（不一致時附差異數字與原因）

### 2.6 單元二檢查清單

- [ ] `dashboard.py` 已生成並成功啟動（瀏覽器顯示儀表板）
- [ ] 完成至少一輪迭代改良，並以 Rerun 重新載入成功
- [ ] 以樞紐分析核對至少一個數字，並抄錄核對結果

## 延伸單元一（選做——建議在家完成）：資料庫化——SQLite

### 3.1 概念：檔案與資料庫

Excel 檔案適合小規模、單人使用的資料分析；資料庫以結構化查詢語言（SQL）存取資料，適合多資料表關聯、多人並用與權限管理——零售企業的營運系統多以資料庫儲存交易資料，分析時再匯出為檔案。本單元以 SQLite 體會此差異：SQLite 為 Python 內建的輕量資料庫引擎，毋須額外安裝。

- SQLite 官方網站（SQL 語法與文件）：官方入口 https://sqlite.org
- Python sqlite3 模組官方文件：官方入口 https://docs.python.org/3/library/sqlite3.html

### 3.2 匯入資料庫並以 SQL 回答管理問題

1. 在同一對話輸入（`<你的用戶名>` 換成你的用戶名）：

```
請將 sales_2026.xlsx 的 orders 工作表匯入 SQLite 資料庫，存為 C:\Users\<你的用戶名>\Documents\hermes-sandbox\lab-ch06\sales.db（資料表命名為 orders）。回報匯入列數、匯入的欄位與你使用的程式碼。
```

**預期**：助手建立 `sales.db` 並回報匯入列數（應與 orders 工作表列數一致：958 列）。

2. 以管理情境提問（每題要求附上 SQL 語句）：

```
請改用 SQL 查詢 sales.db 回答以下問題，每題附上你使用的 SQL 語句：
一、各門市每月的銷售額（輸出表格）
二、哪個商品類別銷售額最高？
三、平均客單價最高的門市是哪一間？
四、哪一個月的銷售額變化最值得管理者注意？請說明你的判讀。
```

**預期**：四個問題均以 SQL 查詢回答，並附 SQL 語句。

3. 體驗重點：同一問題的兩種做法——以樞紐分析操作 Excel 檔案，對照以 SQL 查詢資料庫；記錄兩者之差異。

### 3.3 延伸單元一檢查清單

- [ ] `sales.db` 建立成功，orders 列數與 Excel 一致
- [ ] 至少兩個管理問題由 SQL 回答（附 SQL 語句）
- [ ] 記錄檔案與資料庫之差異觀察

## 延伸單元二（選做——建議在家完成）：MCP 連接器——excel-mcp-server

### 4.1 概念

MCP（Model Context Protocol）為助手調用外部工具的標準協議。excel-mcp-server 為開源 MCP 伺服器，讓助手以工具形式讀寫 Excel 檔案——讀取儲存格、建立樞紐分析、圖表與格式設定；不需要安裝 Microsoft Excel。

- 專案與工具清單：官方入口 https://github.com/haris-musa/excel-mcp-server

### 4.2 配置

1. 於檔案總管位址列貼上 `%USERPROFILE%\Documents\hermes-sandbox\hermes` 後按 Enter；右鍵 `config.yaml` → 開啓方式 → 記事本
2. 按 Ctrl+F 搜索 `mcp_servers`：
   - 已存在：將以下內容新增於該區段之下，縮進層級與既有項目相同
   - 不存在：移至檔案最末尾，另起一行貼入整段（`mcp_servers:` 由第一列開始，不可有空格）
   - 將 `<你的用戶名>` 換成你的用戶名：

```yaml
mcp_servers:
  excel-mcp:
    command: "C:/Users/<你的用戶名>/Documents/hermes-sandbox/hermes/bin/uv.exe"
    args: ["tool", "run", "excel-mcp-server", "stdio"]
```

   縮進規定：`excel-mcp:` 前 2 個空格，`command:` 與 `args:` 前 4 個空格。路徑中的斜線使用 `/`（不是 `\`）。

3. 記事本選「文件 → 保存」，關閉記事本
4. 回到 Hermes 對話，輸入 `/reload-mcp` 並送出
5. 為避免改動原始資料：在檔案總管複製 `sales_2026.xlsx` 為 `sales_2026_mcp.xlsx`（點選檔案按 Ctrl+C，再按 Ctrl+V；對產生的複本按 F2 改名）
6. 驗證——輸入（`<你的用戶名>` 換成你的用戶名）：

```
請用 excel-mcp 工具讀取 C:\Users\<你的用戶名>\Documents\hermes-sandbox\lab-ch06\sales_2026_mcp.xlsx，回報 orders 工作表的欄位名稱與列數。
```

**預期**：助手調用名稱以 `mcp_` 開頭的工具完成讀取，回報欄位名稱與 958 列。首次調用時自動下載伺服器套件，需等候片刻。

7. 進階（可選）：請助手以 MCP 工具在複本上新增樞紐分析表：

```
請用 excel-mcp 工具在 sales_2026_mcp.xlsx 新增一張樞紐分析表工作表：以 store 為列標籤、revenue 加總為值。
```

**預期**：複本檔案出現新工作表；以 Excel 開啓複本檢視結果。

### 4.3 延伸單元二檢查清單

- [ ] `config.yaml` 新增 excel-mcp 並以 `/reload-mcp` 載入
- [ ] 助手成功以 MCP 工具讀取 Excel（回報欄位與列數）
- [ ] （可選）完成一次寫入操作（新增樞紐分析表）

## 小組討論題

1. 儀表板與每週手工整併的靜態報表相比，對管理者的價值何在？請以本次實作經驗舉出至少兩點，並指出一種儀表板仍無法取代人工判斷的情境。
2. 資料品質問題如何影響儀表板上的數字？管理者應在流程的哪一步介入處理——請以本資料集中發現的問題為例說明。
3. AI 生成的儀表板出現數字錯誤時，責任應如何界定（使用者／開發者／管理者）？本次的人工核對流程扮演何種角色？
4. （如完成延伸單元一）以 SQL 查詢資料庫與以樞紐分析操作檔案，各適合什麼情境？請各舉一例說明。

## 常見問題

| 症狀 | 處理 |
|---|---|
| 助手回覆無法讀取 xlsx（顯示 ModuleNotFoundError: openpyxl） | 回覆助手：`請安裝缺少的套件後重試`；或改用 2.3 節之 PowerShell 方式啟動（安裝指令已含 openpyxl） |
| 助手啟動儀表板後長時間沒有回覆 | 長時執行指令屬正常——直接於瀏覽器開啓 http://localhost:8501；仍無畫面則改用 2.3 節之 PowerShell 方式 |
| PowerShell 出現 `streamlit` 相關錯誤且無法啟動 | 重新執行 2.3 節第一行安裝指令（重複執行會以最新設定重新安裝），完成後再執行第二行 |
| 瀏覽器顯示「無法連線」 | 確認 PowerShell 視窗仍保持開啓；確認開啓的埠號與啟動訊息顯示的 `Local URL` 一致 |
| 瀏覽器未自動開啓 | 手動於瀏覽器位址列輸入 http://localhost:8501 |
| 埠 8501 已被占用 | 於第二行指令末尾加上 ` --server.port 8502`，並改以 http://localhost:8502 開啓 |
| 修改 `dashboard.py` 後畫面未更新 | 於瀏覽器畫面點選 Rerun／Always rerun；或重新整理分頁 |
| 找不到 excel-mcp 工具（延伸單元二） | ① 對照 4.2 檢查縮進（2／4 空格）② 確認 `command:` 路徑的 `uv.exe` 確實存在 ③ 再次輸入 `/reload-mcp`，或關閉 Hermes 後重啓 ④ 首次調用需下載套件，確認網絡連線 |
| SQLite 匯入列數與 Excel 不符（延伸單元一） | 請助手說明匯入方式與判讀；對照 orders 工作表列數（958）核對 |
| `:free` 模型回應慢或顯示 429 | 免費層限速屬正常——稍候重試，或更換其他 `:free` 模型 |
| 助手一項品質問題都沒有找到 | 使用 1.2 節之追問指示；助手若把正常資料判為問題，逐項追問其判斷理由 |

## 資料與工具速查

| 用途 | 位置 |
|---|---|
| 本實作工作資料夾 | `C:\Users\<你的用戶名>\Documents\hermes-sandbox\lab-ch06\` |
| 儀表板（本機） | 啟動後於瀏覽器開啓 http://localhost:8501 |
| Hermes Agent 官方文件 | https://hermes-agent.nousresearch.com/docs |
| Streamlit 官方文件（安裝與元件） | https://docs.streamlit.io |
| excel-mcp-server 專案 | https://github.com/haris-musa/excel-mcp-server |
| SQLite／Python sqlite3 文件 | https://sqlite.org ； https://docs.python.org/3/library/sqlite3.html |
| Hermes 本機設定（config.yaml） | `C:\Users\<你的用戶名>\Documents\hermes-sandbox\hermes\config.yaml` |
| 工具無法使用時 | 查 AI 工具切換對照指南（已發佈 GitHub：aiccuser1/msba6125-2026 lab/ai-tools-guide.md；Gemini↔Copilot↔WPS AI／DeepSeek 思考模式／harness 說明） |
