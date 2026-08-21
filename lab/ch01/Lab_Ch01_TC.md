---
type: lab
tags:
  - teaching
  - msba6125
  - lab
  - guide
created: 2026-08-19
updated: 2026-08-21
---

# 第 1 課｜實驗環境部署與提示詞基礎

> 本單元包含兩項核心學習任務：（1）依六步流程完成個人實驗環境之部署，須於下次課前完成；（2）習得提示詞（prompt）之基礎概念，並藉由對話式 AI 代理（conversational AI agent）完成一篇指定課程文獻之翻譯——該任務旨在融合文本閱讀與自主探究式學習。

## 學習目標（Learning Objectives）

完成本單元后，你將具備：

- 部署個人實驗環境之能力（Google 帳號、Gemini API 金鑰、Colab、GitHub 帳號、NotebookLM）
- 提示詞三要素（明確性 specific／角色 role／格式 format）之理解與應用
- 以「AI 翻譯＋人工校對」工作流完成英文文獻閱讀之方法

## 課前準備（Pre-class Requirement）

- 一臺可連網之電腦或手機
- 一個有效信箱（用於帳號註冊）

## 第一部分：實驗環境部署（六步協議，課前完成）

以下六步依據各平臺官方最新手冊編寫（以 2026-08 爲準；平臺規則可能調整，如遇變動以官方最新說明爲準）。

### 步驟 1：註冊 Google 帳號

- 用途：Gemini 網頁版、Colab 與 NotebookLM 均需 Google 帳號登入
- 官方指引：support.google.com/accounts/answer/27441（Google 帳號說明中心）
- 操作：訪問 accounts.google.com/signup → 填寫姓名、使用者名稱（作爲信箱）、密碼 → 依提示完成驗證
- 已有 Google 帳號者跳過此步

### 步驟 2：取得 Gemini API 金鑰（API key）

- 用途：後續課程以 Colab 程式呼叫 Gemini 模型；網頁版對話無需金鑰
- 官方指引：ai.google.dev/gemini-api/docs/api-key（Gemini API 官方文件「API keys」）
- 操作：訪問 aistudio.google.com/apikey → 以 Google 帳號登入 → 點選「Create API key」→ 複製並妥善保存
- 說明：免費層無需綁定信用卡；金鑰僅限個人使用，不得外傳

### 步驟 3：Colab 首次執行

- 用途：雲端 Python 筆記（notebook）執行環境
- 官方入口：colab.research.google.com
- 操作：訪問上述網址 → 以 Google 帳號登入 → 開啓任一範例 notebook → 依序點擊各程式單元（cell）左側執行鈕 → 確認輸出正常
- 說明：免費層資源與用量有限制；本課程每課請求量小，免費層足夠

### 步驟 4：註冊 GitHub 帳號

- 用途：課程實驗程式（notebook）經 GitHub 發佈與取得
- 官方指引：docs.github.com（GitHub 文件「Creating an account on GitHub」）
- 操作：訪問 github.com/signup → 填寫信箱、密碼、使用者名稱 → 完成信箱驗證
- 說明：下載公開倉庫無需登入；建議註冊以便保存個人副本

### 步驟 5：驗證「GitHub → Colab」鏈路

- 用途：驗證課程實驗程式之取得流程
- 操作：
  1. 確認瀏覽器已登入你的 Google 賬號（未登入時 Colab 會要求先登入——必須登入才能開啓與執行）
  2. 於瀏覽器開啓以下網址（Colab 直接加載 GitHub 倉庫中的 notebook）：
     https://colab.research.google.com/github/aiccuser1/msba6125-2026/blob/main/lab/ch01/Lab_Ch01.ipynb
  3. Colab 開啓後（以你的賬號工作階段載入）：選單 File（檔案）→ Save a copy in Drive（在雲端硬碟另存副本——否則只是臨時載入，不會保存）
  4. 於個人副本中依序執行各單元（環境驗證＋基本運算＋Gemini API 連線測試），確認輸出顯示 OK
- 說明：一律操作個人副本，不修改原檔；API 金鑰以 Colab Secrets 儲存（notebook 內含設定說明）

### 步驟 6：啓用 NotebookLM（Gemini Notebook）

- 用途：知識庫問答工具——以課程文件爲基礎提問，答案附原文出處；本課程指定使用 NotebookLM 進行文獻閱讀與知識庫建置
- 說明：NotebookLM 已更名爲 **Gemini Notebook**（介面與功能相同，官方新名稱）；登入 notebooklm.google.com 後即可使用
- 操作：訪問 notebooklm.google.com → 以 Google 帳號登入 → 新建筆記本 → 上傳課程講義（PDF／Word）→ 基於文件提問，答案附原文出處
- 無法存取 Google 服務者：可使用替代工具（見文末「工具對照表」——如元寶＋ima 桌面版），用法與 NotebookLM 相同

## 第二部分：提示詞基礎（三要素）

提示詞（prompt）爲你對 AI 之指令。有效提示詞包含三項要素：

| 要素 | 定義 | 反例 → 正例 |
|---|---|---|
| 明確性（specific） | 明定任務對象、範圍與約束 | 「幫我翻譯這段」→「將下列英文段落翻譯爲簡體中文，術語保留英文原文並於括號內註明」 |
| 角色（role） | 指定 AI 之身份／視角 | 「翻譯一下」→「你是一位專業譯員，擅長管理資訊系統領域之中英互譯」 |
| 格式（format） | 指定輸出之組織方式 | 「總結一下」→「以三個要點條列，每點不超過 50 字」 |

> 原則：同一任務，提示詞越具體，輸出越可用。此爲後續各單元之核心技能。

## 第三部分：應用實踐——以 AI 翻譯完成指定閱讀

課程於 Canvas 提供每章閱讀材料（英文原文）。本單元藉對話式 AI 代理完成閱讀：

1. 開啓 Canvas 課程頁，下載 **Reading_Ch01** 任一篇文章（英文原文）
2. 開啓 Gemini 網頁版（gemini.google.com）或其他對話式 AI 代理（元寶／DeepSeek／Kimi 等）
3. 複製文章第一段，輸入以下提示詞：

```
你是一位專業譯員，擅長管理資訊系統領域的中英互譯。
請將以下英文段落翻譯爲簡體中文：
- 術語首次出現時保留英文原文並於括號內註明
- 保持學術語氣
- 直譯優先，不意譯

[貼上文章段落]
```

4. 對照譯文與原文，檢核：術語是否保留？語句是否通順？有疑問處再次提問（如「此句之 out-of-bounds read 爲何意？」）
5. 重複步驟 3-4，完成整篇文章之翻譯閱讀

**完成標準**：能以自身語言向同學複述該文要點（不要求繳交譯稿——翻譯爲閱讀之手段，非交付物）。

## 課後完成清單

- [ ] 註冊 Google 帳號（或確認已有）
- [ ] 取得 Gemini API 金鑰並妥善保存
- [ ] Colab 首次執行成功
- [ ] 註冊 GitHub 帳號並完成信箱驗證
- [ ] 自課程倉庫 `lab/ch01/` 成功開啓 notebook → Save a copy → 執行成功
- [ ] 啓用 NotebookLM（Gemini Notebook）並上傳一份講義試用
- [ ] 以對話式 AI 代理完成至少一段 Reading_Ch01 文章之翻譯閱讀

## 工具對照表

本課程以 Google 生態（Gemini＋Colab＋NotebookLM）爲主。任一工具無法使用時（如網路環境受限），按下表切換替代工具：

| 用途 | 主力工具 | 替代工具 |
|---|---|---|
| 對話／翻譯 | Gemini（gemini.google.com） | 元寶（yuanbao）／DeepSeek／Kimi／WPS AI |
| 知識庫問答 | NotebookLM（Gemini Notebook） | 元寶＋ima 桌面版 |
| 程式執行 | Colab | 本地 Python 環境 |
| 文書處理 | Google Docs | Word＋WPS AI |

各平臺免費額度與規則會調整，以官方最新說明爲準。本課程練習請求量小（每課數條 prompt），免費檔足夠。

> 替代工具之詳細 prompt 對照與 DeepSeek 思考模式說明，見課程 GitHub 倉庫 `lab/ai-tools-guide.md`（可選閱讀）。
