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

# 第 6 课｜管理者自动仪表板：以 Hermes 连接销售资料

> 本实作由两个核心单元与两个延伸单元组成，情境为虚构咖啡连锁「半岛咖啡」（Peninsula Coffee，共三间门市）：你的角色为分析人员，为门市管理者建置每周检视的销售仪表板。单元一为资料理解与指标定义：以 Hermes 读取销售资料（Excel 工作簿），输出栏位结构、资料规模与资料品质观察，并与助手共同定义仪表板的关键指标（KPI）。单元二为仪表板建置与人工核对：由助手生成 Python（Streamlit）仪表板程式并于本机启动，在浏览器检视、迭代改良，最后以 Excel 枢纽分析完成核实。延伸单元（选做，建议在家完成）：其一为将资料汇入 SQLite 资料库，体会档案与资料库的差异；其二为以 MCP 连接器（excel-mcp-server）扩充助手直接操作 Excel 的能力。每个单元结束均有检查清单，以供核对完成度。

## 学习目标（Learning Objectives）

完成本实作后，你将具备：

- 以 AI 助手读取多工作表 Excel 资料，说明其栏位结构、资料规模与资料品质问题
- 定义管理者仪表板之关键指标，说明各指标的计算方式与管理意义
- 由助手生成 Streamlit 仪表板程式，于本机启动并在浏览器检视
- 以既有对话追加指示之方式迭代改良仪表板输出
- 以 Excel 枢纽分析对照仪表板数字，完成人工核实
- 说明档案（Excel）与资料库（SQLite）在资料存取上的差异
- （延伸）以 MCP 连接器扩充助手对 Excel 档案之操作能力

## 课前准备（Pre-class Requirement）

1. 完成第 5 课实作：Hermes Desktop 已安装，并以 Nous Portal 免费方案连接模型；本实作沿用安装位置 `C:\Users\<你的用户名>\Documents\hermes-sandbox`
2. 确认 Hermes 使用之模型名称以 `:free` 结尾；若出现「并非为工具使用设计」之提示，于模型设定改选其他 `:free` 模型
3. 从课程页（Canvas 档案区）下载资料档 `sales_2026.xlsx`（放置位置见 1.1 节）
4. 资料为课程生成之虚构零售资料，不含任何真实客户或公司资讯

## 单元一：资料理解与指标定义

### 1.1 建立工作资料夹

1. 开启档案总管（工作列上的资料夹图示，或按键盘 Windows 键＋E）
2. 于上方位址列贴上 `C:\Users\<你的用户名>\Documents\hermes-sandbox` 后按 Enter——此为第 5 课建立的 Hermes 资料位置
3. 于资料夹空白处按右键 → 新增 → 资料夹；输入名称 `lab-ch06` 后按 Enter 确认
4. 将已下载的 `sales_2026.xlsx` 移入 `C:\Users\<你的用户名>\Documents\hermes-sandbox\lab-ch06\`

**预期**：资料夹 `lab-ch06` 内有 `sales_2026.xlsx` 一个档案。

### 1.2 资料理解：请助手读取资料

资料分析的第一步是确认资料的栏位、规模与品质——资料理解先于指标定义与图表设计。

1. 开启 Hermes Desktop；在对话框输入以下文字并送出（路径中 `<你的用户名>` 换成你的 Windows 用户名）：

```
请读取 C:\Users\<你的用户名>\Documents\hermes-sandbox\lab-ch06\sales_2026.xlsx，回报：
一、各工作表的名称与用途
二、每张工作表的栏位、资料列数与时间范围
三、你观察到的资料品质问题（缺值、重复、极端值、命名不一致等，逐项列出）
请只读取，不要修改或清理档案。
```

**预期**：助手以交易表（orders）为主体回报——列出其栏位（订单编号、日期、门市、商品、数量、单价、成本、营收、付款方式等）、规模（九百余列）与时间范围（2026 年 1 月至 9 月），并列出至少一项资料品质观察。

2. 若助手未提及任何品质问题，追问：

```
请再逐项检查：订单列是否完全无重复、门市名称是否完全一致、数量与单价是否有不合理数值、必填栏位是否有空白。逐项回答检查结果。
```

**预期**：助手逐项回答检查结果。将发现的品质问题抄录下来。

### 1.3 指标定义：与助手共同设计仪表板

管理仪表板呈现什么，取决于管理者每周要回答什么问题。指标定义先于图表设计。

1. 在同一对话输入：

```
你是协助零售连锁管理者的资料分析师。根据刚才的资料，请设计管理者每周检视的仪表板指标清单，涵盖：
一、总销售额、订单数、平均客单价
二、月度销售趋势
三、热销商品与类别营收分布
四、各门市月度目标达成率（对照 targets 工作表）
每个指标请说明：计算方式、建议图表类型、管理意义（管理者用来回答什么问题）。
最后补充一至两个你认为值得加入的指标，并说明理由。
```

2. 阅读回复后，记录 KPI 清单。可请助手将清单整理为表格：`请把上述指标整理成表格：指标、计算方式、图表类型、管理意义。`

**预期**：产出一份 KPI 清单，每项含计算方式与管理意义。

### 1.4 单元一检查清单

- [ ] 助手完成资料理解回报（工作表清单、orders 栏位与规模、时间范围）
- [ ] 已抄录至少一项资料品质问题
- [ ] 已定义仪表板 KPI 清单（含计算方式与管理意义）

## 单元二：仪表板建置、启动与人工核对

### 2.1 概念：管理者仪表板与 Streamlit

管理者仪表板将关键指标集中於单一画面，取代每周手工整并的静态报表。静态报表每次更新均需手工重做；仪表板由程式读取资料档绘制画面——此处「自动」的含义是：程式每次启动或重新整理时重新读取资料档，资料档更新后，画面即呈现最新数字，无需重新制作报表。

仪表板需以可于浏览器检视的互动程式实作；本实作采用 Streamlit——一套以 Python 撰写资料应用的开源框架：单一程式档即可产生含筛选器、图表与表格的网页介面，于本机浏览器执行，毋须部署伺服器。单一程式档之形式亦便于由助手生成与迭代修改。

- Streamlit 官方文件（元件与 API 参考）：官方入口 https://docs.streamlit.io
- 官方安装指引：官方入口 https://docs.streamlit.io/get-started/installation

### 2.2 生成仪表板程式

在同一对话输入（延续单元一之对话；`<你的用户名>` 换成你的用户名）：

```
请用 Python 与 Streamlit 撰写仪表板程式，存为 C:\Users\<你的用户名>\Documents\hermes-sandbox\lab-ch06\dashboard.py，需求：
一、读取同资料夹的 sales_2026.xlsx（orders 与 targets 工作表）
二、画面上方显示三个指标：总销售额、订单数、平均客单价
三、月度销售趋势折线图
四、热销商品前 10 名与类别营收分布图
五、各门市月度目标达成率表格
六、侧栏提供门市与日期范围筛选器
完成后回报：程式档案路径、读取的资料栏位、以及你对资料的任何处理决定。
若执行环境缺少套件（如 pandas、openpyxl），请先安装后继续。
```

**预期**：助手回报已建立 `dashboard.py`。于档案总管开启 `lab-ch06` 资料夹，可见 `dashboard.py` 与 `sales_2026.xlsx` 两个档案。

### 2.3 启动仪表板

以 PowerShell 启动：

1. 开启 Windows PowerShell（开始功能表搜寻 PowerShell；不需要管理员模式）
2. 贴上以下第一行并按 Enter；等候安装完成，再贴第二行并按 Enter：

```powershell
& "$HOME\Documents\hermes-sandbox\hermes\bin\uv.exe" tool install streamlit --with openpyxl
& "$HOME\.local\bin\streamlit.exe" run "$HOME\Documents\hermes-sandbox\lab-ch06\dashboard.py"
```

**预期**：第一行显示安装过程，最后出现 `Installed 1 executable: streamlit`；第二行显示 `Local URL: http://localhost:8501`，浏览器自动开启仪表板分页。

- 浏览器未自动开启时：手动开启浏览器，于位址列输入 `http://localhost:8501`
- 此 PowerShell 视窗须保持开启——关闭视窗即停止仪表板

3. （替代方式）由助手启动——在同一对话输入：

```
请以背景方式启动刚生成的仪表板（不要让命令长时间占住执行），完成后告诉我在哪个网址检视。
```

**预期**：助手回报检视网址（通常为 http://localhost:8501）。若助手持续执行而没有回复：直接于浏览器开启 `http://localhost:8501` 检视；仍无画面则改用上述 PowerShell 方式。

### 2.4 迭代改良

在 Hermes 同一对话追加指示（以下为示例，可依你的 KPI 清单调整）：

```
仪表板可以正常检视。请追加改良：
一、页面标题改为「半岛咖啡 门市周报仪表板」
二、月度趋势图加上每月数字标签
三、类别营收分布改为圆饼图
完成后说明你修改了哪些部分。
```

**预期**：助手回报修改完成。回到浏览器分页——画面提示 `Source file changed` 时点选 **Rerun**（或 **Always rerun**）；或直接重新整理分页，画面即呈现改良后版本。

若改良后画面报错：将错误讯息整段贴回对话，请助手修正（在同一对话继续，不要另开新对话）。

### 2.5 人工核对：Excel 枢纽分析

AI 产出的数字须人工抽查核实。以 Excel 枢纽分析独立计算同一指标，与仪表板对照。

1. 以 Excel 开启 `sales_2026.xlsx`，切到 `orders` 工作表
2. 点选资料区任一储存格 → 功能区「插入」→「枢纽分析表」→ 于对话框按「确定」（预设置于新工作表）
3. 于枢纽分析表栏位窗格：将 `store` 拖至「列」区域；将 `revenue` 拖至「值」区域（确认显示「加总」）
4. 得到各门市销售额后，与仪表板的数字并排比较

**预期**：枢纽数字与仪表板一致。若助手在程式中对资料做了额外处理（如排除特定资料列），可能出现差异——记录差异，并回到对话追问助手原因。

5. 抄录核对结果：一致／不一致（不一致时附差异数字与原因）

### 2.6 单元二检查清单

- [ ] `dashboard.py` 已生成并成功启动（浏览器显示仪表板）
- [ ] 完成至少一轮迭代改良，并以 Rerun 重新载入成功
- [ ] 以枢纽分析核对至少一个数字，并抄录核对结果

## 延伸单元一（选做——建议在家完成）：资料库化——SQLite

### 3.1 概念：档案与资料库

Excel 档案适合小规模、单人使用的资料分析；资料库以结构化查询语言（SQL）存取资料，适合多资料表关联、多人并用与权限管理——零售企业的营运系统多以资料库储存交易资料，分析时再汇出为档案。本单元以 SQLite 体会此差异：SQLite 为 Python 内建的轻量资料库引擎，毋须额外安装。

- SQLite 官方网站（SQL 语法与文件）：官方入口 https://sqlite.org
- Python sqlite3 模组官方文件：官方入口 https://docs.python.org/3/library/sqlite3.html

### 3.2 汇入资料库并以 SQL 回答管理问题

1. 在同一对话输入（`<你的用户名>` 换成你的用户名）：

```
请将 sales_2026.xlsx 的 orders 工作表汇入 SQLite 资料库，存为 C:\Users\<你的用户名>\Documents\hermes-sandbox\lab-ch06\sales.db（资料表命名为 orders）。回报汇入列数、汇入的栏位与你使用的程式码。
```

**预期**：助手建立 `sales.db` 并回报汇入列数（应与 orders 工作表列数一致：958 列）。

2. 以管理情境提问（每题要求附上 SQL 语句）：

```
请改用 SQL 查询 sales.db 回答以下问题，每题附上你使用的 SQL 语句：
一、各门市每月的销售额（输出表格）
二、哪个商品类别销售额最高？
三、平均客单价最高的门市是哪一间？
四、哪一个月的销售额变化最值得管理者注意？请说明你的判读。
```

**预期**：四个问题均以 SQL 查询回答，并附 SQL 语句。

3. 体验重点：同一问题的两种做法——以枢纽分析操作 Excel 档案，对照以 SQL 查询资料库；记录两者之差异。

### 3.3 延伸单元一检查清单

- [ ] `sales.db` 建立成功，orders 列数与 Excel 一致
- [ ] 至少两个管理问题由 SQL 回答（附 SQL 语句）
- [ ] 记录档案与资料库之差异观察

## 延伸单元二（选做——建议在家完成）：MCP 连接器——excel-mcp-server

### 4.1 概念

MCP（Model Context Protocol）为助手调用外部工具的标准协议。excel-mcp-server 为开源 MCP 伺服器，让助手以工具形式读写 Excel 档案——读取储存格、建立枢纽分析、图表与格式设定；不需要安装 Microsoft Excel。

- 专案与工具清单：官方入口 https://github.com/haris-musa/excel-mcp-server

### 4.2 配置

1. 于档案总管位址列贴上 `%USERPROFILE%\Documents\hermes-sandbox\hermes` 后按 Enter；右键 `config.yaml` → 开启方式 → 记事本
2. 按 Ctrl+F 搜索 `mcp_servers`：
   - 已存在：将以下内容新增于该区段之下，缩进层级与既有项目相同
   - 不存在：移至档案最末尾，另起一行贴入整段（`mcp_servers:` 由第一列开始，不可有空格）
   - 将 `<你的用户名>` 换成你的用户名：

```yaml
mcp_servers:
  excel-mcp:
    command: "C:/Users/<你的用户名>/Documents/hermes-sandbox/hermes/bin/uv.exe"
    args: ["tool", "run", "excel-mcp-server", "stdio"]
```

   缩进规定：`excel-mcp:` 前 2 个空格，`command:` 与 `args:` 前 4 个空格。路径中的斜线使用 `/`（不是 `\`）。

3. 记事本选「文件 → 保存」，关闭记事本
4. 回到 Hermes 对话，输入 `/reload-mcp` 并送出
5. 为避免改动原始资料：在档案总管复制 `sales_2026.xlsx` 为 `sales_2026_mcp.xlsx`（点选档案按 Ctrl+C，再按 Ctrl+V；对产生的复本按 F2 改名）
6. 验证——输入（`<你的用户名>` 换成你的用户名）：

```
请用 excel-mcp 工具读取 C:\Users\<你的用户名>\Documents\hermes-sandbox\lab-ch06\sales_2026_mcp.xlsx，回报 orders 工作表的栏位名称与列数。
```

**预期**：助手调用名称以 `mcp_` 开头的工具完成读取，回报栏位名称与 958 列。首次调用时自动下载伺服器套件，需等候片刻。

7. 进阶（可选）：请助手以 MCP 工具在复本上新增枢纽分析表：

```
请用 excel-mcp 工具在 sales_2026_mcp.xlsx 新增一张枢纽分析表工作表：以 store 为列标签、revenue 加总为值。
```

**预期**：复本档案出现新工作表；以 Excel 开启复本检视结果。

### 4.3 延伸单元二检查清单

- [ ] `config.yaml` 新增 excel-mcp 并以 `/reload-mcp` 载入
- [ ] 助手成功以 MCP 工具读取 Excel（回报栏位与列数）
- [ ] （可选）完成一次写入操作（新增枢纽分析表）

## 小组讨论题

1. 仪表板与每周手工整并的静态报表相比，对管理者的价值何在？请以本次实作经验举出至少两点，并指出一种仪表板仍无法取代人工判断的情境。
2. 资料品质问题如何影响仪表板上的数字？管理者应在流程的哪一步介入处理——请以本资料集中发现的问题为例说明。
3. AI 生成的仪表板出现数字错误时，责任应如何界定（使用者／开发者／管理者）？本次的人工核对流程扮演何种角色？
4. （如完成延伸单元一）以 SQL 查询资料库与以枢纽分析操作档案，各适合什么情境？请各举一例说明。

## 常见问题

| 症状 | 处理 |
|---|---|
| 助手回复无法读取 xlsx（显示 ModuleNotFoundError: openpyxl） | 回复助手：`请安装缺少的套件后重试`；或改用 2.3 节之 PowerShell 方式启动（安装指令已含 openpyxl） |
| 助手启动仪表板后长时间没有回复 | 长时执行指令属正常——直接于浏览器开启 http://localhost:8501；仍无画面则改用 2.3 节之 PowerShell 方式 |
| PowerShell 出现 `streamlit` 相关错误且无法启动 | 重新执行 2.3 节第一行安装指令（重复执行会以最新设定重新安装），完成后再执行第二行 |
| 浏览器显示「无法连线」 | 确认 PowerShell 视窗仍保持开启；确认开启的埠号与启动讯息显示的 `Local URL` 一致 |
| 浏览器未自动开启 | 手动于浏览器位址列输入 http://localhost:8501 |
| 埠 8501 已被占用 | 于第二行指令末尾加上 ` --server.port 8502`，并改以 http://localhost:8502 开启 |
| 修改 `dashboard.py` 后画面未更新 | 于浏览器画面点选 Rerun／Always rerun；或重新整理分页 |
| 找不到 excel-mcp 工具（延伸单元二） | ① 对照 4.2 检查缩进（2／4 空格）② 确认 `command:` 路径的 `uv.exe` 确实存在 ③ 再次输入 `/reload-mcp`，或关闭 Hermes 后重启 ④ 首次调用需下载套件，确认网络连线 |
| SQLite 汇入列数与 Excel 不符（延伸单元一） | 请助手说明汇入方式与判读；对照 orders 工作表列数（958）核对 |
| `:free` 模型回应慢或显示 429 | 免费层限速属正常——稍候重试，或更换其他 `:free` 模型 |
| 助手一项品质问题都没有找到 | 使用 1.2 节之追问指示；助手若把正常资料判为问题，逐项追问其判断理由 |

## 资料与工具速查

| 用途 | 位置 |
|---|---|
| 本实作工作资料夹 | `C:\Users\<你的用户名>\Documents\hermes-sandbox\lab-ch06\` |
| 仪表板（本机） | 启动后于浏览器开启 http://localhost:8501 |
| Hermes Agent 官方文件 | https://hermes-agent.nousresearch.com/docs |
| Streamlit 官方文件（安装与元件） | https://docs.streamlit.io |
| excel-mcp-server 专案 | https://github.com/haris-musa/excel-mcp-server |
| SQLite／Python sqlite3 文件 | https://sqlite.org ； https://docs.python.org/3/library/sqlite3.html |
| Hermes 本机设定（config.yaml） | `C:\Users\<你的用户名>\Documents\hermes-sandbox\hermes\config.yaml` |
| 工具无法使用时 | 查 AI 工具切换对照指南（已发布 GitHub：aiccuser1/msba6125-2026 lab/ai-tools-guide.md；Gemini↔Copilot↔WPS AI／DeepSeek 思考模式／harness 说明） |
