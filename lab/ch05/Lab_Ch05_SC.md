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

# 第 5 课｜Hermes Agent 安装与首次商务任务

> 本实作为 Hermes 平台线第一课，由两个单元组成。单元一在你的电脑安装本地 AI 助手 Hermes Desktop，以 Nous Portal 免费方案连接模型，并完成首次对话与文件权限测试——**建议课前在家完成**（安装需下载数百 MB，在家完成可让课堂时间用于任务与讨论）。单元二为课堂任务：以助手完成两项日常商务任务，练习「在同一对话追加指示」的迭代方式，最后进行小组讨论。

## 学习目标（Learning Objectives）

完成本实作后，你将具备：

- 在自己的电脑安装 Hermes Desktop，并以免费方案（Nous Portal Free plan）连接免费模型
- 说明 agentic AI 助手与单纯聊天机器人的差异：工具、记忆与自主执行
- 以文件存取授权为实例，说明本地 AI 助手的权限管理观念
- 与助手协作完成邮件、会议与行程三类商务任务中的两项
- 输出不理想时，在既有对话中追加指示改良输出，而非另开对话重新开始

## 课前准备（Pre-class Requirement）

1. 电脑：Windows 10／11（本手册以 Windows 为基准）；磁碟可用空间不少于 10 GB
2. 可用的 Google 或 GitHub 帐号（可完成浏览器登入）
3. **建议先在家中完成单元一**：安装需下载数百 MB 至 1 GB 级档案，视网络约 5–15 分钟。若未及在家完成：课堂上即时安装（约 10–20 分钟）；时间不足时可先与同组已完成安装者共用一台机器，并于课后在家补完
4. 若使用 VPN 或代理且下载、连线缓慢：先关闭后重试（见下方「常见问题」）

## 单元一：安装 Hermes Desktop 与连接免费模型（建议课前在家完成）

### 1.1 概念：什么是 Agentic AI

AI 助手（agent）与单纯聊天机器人的差别在于三件事：**工具**（可以读写文件、搜索网页、查询数据库）、**记忆**（跨对话保留你的设定与偏好）与**自主执行**（依你的指示完成多步骤任务，而非只回一段文字）。Hermes Agent 是一套安装于你电脑本地的 AI 助手框架，由 Nous Research 开发：你的对话资料与设定存放于本机，模型连线则经由你选择的云端服务（本课使用免费的 Nous Portal 方案）。

官方概念与快速入门见 Hermes Agent 官方文件：官方入口 https://hermes-agent.nousresearch.com/docs/getting-started/quickstart 。

### 1.2 下载与安装

1. 浏览器开启官方下载页：官方入口 https://hermes-agent.nousresearch.com/desktop
2. 按 **Windows** 下载按钮，取得安装档 `Hermes-Setup.exe`（档案由 Nous Research Inc. 签名）。
3. 于下载资料夹双击执行。**预期画面**：
   - 若出现 Windows SmartScreen「Windows 已保护您的电脑」：按 **更多信息** → **仍要运行**（首次运行新软件属正常防护，安装档有签名）
   - 主画面按 **INSTALL** 开始安装
   - 画面显示安装进度（自动安装 Python、Node.js、Git 与 Hermes 核心，并建立虚拟环境）——期间不要关闭窗口或让电脑休眠
   - 安装失败时记下错误文字，见下方「常见问题」之「安装失败」条目处理
4. **安装完成后先不要按 LAUNCH**：先依 1.3 节把 Hermes 资料夹移至课程统一位置（首次启动前搬移，可避免档案占用问题），再回来启动。
5. （可选）安装后检查：开始功能表搜寻 **PowerShell** → 开启 Windows PowerShell → 执行 `hermes --version`。**预期输出**：一行版本号（如 `hermes 0.21.0`）。若显示「无法将 hermes 识别为 cmdlet」：关闭全部 PowerShell 后开新窗口重试；仍失败表示安装器未把 hermes 加入 PATH——Desktop 使用不受影响，可跳过此检查。

### 1.3 搬移至课程统一位置

后续操作一律以 `Documents\hermes-sandbox\hermes` 为 Hermes 资料位置。搬移以「移动＋连结」方式进行：资料实际存放于新位置，安装器原位置保留为连结——程式运作、PATH 设定、登入状态均不受影响。

1. 确认 Hermes 未在执行（尚未按 LAUNCH 者无此问题）。
2. 开启 **Windows PowerShell**（开始功能表搜寻 PowerShell，不需要管理员模式），依序贴上执行以下三行（每行按 Enter 完成后再贴下一行）：

```powershell
New-Item -ItemType Directory -Force "$HOME\Documents\hermes-sandbox"
Move-Item "$env:LOCALAPPDATA\hermes" "$HOME\Documents\hermes-sandbox\hermes"
New-Item -ItemType Junction -Path "$env:LOCALAPPDATA\hermes" -Target "$HOME\Documents\hermes-sandbox\hermes"
```

**预期**：三行均无错误讯息。搬移后 Hermes 资料实际存放于 `C:\Users\<你的用户名>\Documents\hermes-sandbox\hermes`（「资料与工具速查」以此位置为准）；安装器原位置 `%LOCALAPPDATA%\hermes` 仍指向同一份资料。若第一行出现「已存在」提示或第三行报「已存在」错误：表示先前已搬移过，可直接跳到下一步。

3. 回到安装器视窗按 **LAUNCH** 启动（安装视窗已关闭者：由开始功能表或桌面捷径开启 Hermes）。

遇到搬移错误（档案被占用或拒绝存取）：关闭所有 Hermes 视窗，并于系统匣（工作列右下角）的 Hermes 图示按右键选「结束」，再重跑该三行指令。

### 1.4 连接免费模型（Nous Portal）

1. 按 **LAUNCH**（或桌面捷径）启动 Hermes Desktop。
2. **预期画面**：出现设定画面 "Let's get you set up"——选择 **Nous Portal**（标示 Recommended）。
3. 浏览器自动开启 Nous Portal 登入页（官方入口 https://portal.nousresearch.com ）：
   - 没有帐号者按 **Sign up**；已有帐号者直接登录。登录方式以当日页面提供者为准，**Google 或 GitHub 皆可**
   - 选择 **Free plan**
   - 按 **SUBSCRIBE AND CONNECT**。若页面出现 Stripe 信用卡输入栏：免费方案不需要信用卡——卷到方案列表下方找 **CONNECT** 按钮
   - 页面显示 "CONNECTED"（或授权成功信息）后可关闭浏览器
4. 回到 Hermes Desktop（会自动接续）：「Default model」卡片按 **Change** → 搜索框输入 **free** → 选择名称以 `:free`／`:Free` 结尾的模型——免费模型名单会轮换，以当日显示为准；**课堂上请各自选择不同的 `:free` 模型**（分散负载，避免全班集中使用同一模型）。
5. 若选定后 Hermes 对该模型显示「并非为工具使用设计」类警示：改选其他 `:free` 模型。
6. 按 **Start chatting** 进入对话——前往 1.5 节。

### 1.5 首次对话与介面认识

1. 在输入框输入以下文字并送出：`你好，请介绍你自己，并说明你目前使用的模型与 provider。`
   **预期**：助手正常回复，且回复中提到当前模型名称。视窗右下角状态列亦显示模型名称，可与回复对照。
2. 认识界面：窗口左侧边栏可见 agents／skills／memory 等分区——这些是助手的工具与记忆存放区；本单元不需要改动，先观察即可。

### 1.6 文件存取权限测试

在输入框输入：`请在我的「文档」（Documents）资料夹建立一个名为 hermes-test 的资料夹，并在里面写一个 test.txt 档案，内容写「hello from hermes」。`

**预期**：助手会请求文件写入权限——**权限请求出现时先检视范围，并决定是否批准**（本地 AI 助手可存取本机档案，权限应以最小范围为原则；本任务授予 Documents 范围即可）。完成后，于档案总管开启 `C:\Users\<你的用户名>\Documents\hermes-test\test.txt`，应看到档案存在且内容正确。

 **记录**：权限请求视窗显示的范围，以及批准或缩小范围之决定。

若助手回复没有权限机制或直接拒绝：改以更小范围再试一次，如 `请在桌面上建立一个 test.txt，写入「hello」`。

### 1.7 单元一检查清单

- [ ] 安装完成并通过首次对话（回复与状态列可见模型名称）
- [ ] Hermes 资料位于 `Documents\hermes-sandbox\hermes`（可于档案总管确认）
- [ ] 文件存取权限请求出现并完成处理——记录请求范围与处理方式

### 1.8 安全与用量注意

- 免费方案不等于无限：有限速机制，回应慢或暂时拒绝属正常——稍候重试，或按视窗右下角模型名称更换其他 `:free` 模型
- 你的对话会送往云端模型：只使用虚构与公开资料；不要输入个人密码、证件号码或任何敏感资料
- API key 与登录状态存放于本机（`Documents\hermes-sandbox\hermes`）：共用电脑使用后登出为宜；不要把 `config.yaml` 或 `.env` 传给他人
- 本阶段**不连接** WhatsApp／Telegram 等个人讯息平台——个人讯息帐号的 gateway 串接属后续主题，避免课堂示范误传真实讯息

## 单元二：课堂任务——日常商务应用

### 2.1 开场确认

1. 开启 Hermes Desktop；未完成单元一者：先依 1.2–1.4 节完成安装与连接（课堂安装约 10–20 分钟）。
2. 确认视窗右下角状态列显示你所选的 `:free` 模型名称；不确定时输入 `你好，请说明你目前使用的模型。` 核对。

### 2.2 商务任务（三项选两项）

以下任务全部使用**虚构资料**：免费模型多为云端路由，你输入的内容会送往第三方云端模型，因此不要使用任何真实客户、同事或公司资料。完成每个任务后，若输出不满意，**在同一个对话中追加指示改良输出**（例如「把第二段改得更简短」「加一列状态栏」），不要另开新对话重新开始。

**任务 A：客户邮件草稿**

输入：`我是一家小型咖啡豆供应商的客服。请以简体中文草拟一封回复邮件，对象是投诉「上周订单延迟三天、且缺少一包 500g 耶加雪菲」的客户王先生。回复需包含：道歉、原因说明（物流延误，虚构）、补寄方案、下次订单 9 折补偿。语气专业诚恳，不超过 200 字。`

**任务 B：会议纪要整理**

输入：`以下是虚构的会议零散记录，请整理成会议纪要，包含：决议、行动事项（负责人＋期限＋状态栏）与未决问题。记录：「Amy 说市场部下月预算砍 15%；Ben 提出改投短视频渠道；大家同意先做两周小规模测试；Carol 负责出测试方案，周五前；预算数字还要和财务对；下周一例会复审」。`

**任务 C：一周行程安排**

输入：`我是虚构的销售经理。请为我安排下周一到周五的行程：每天上午留 1 小时处理邮件；周二和周四下午各有 2 小时客户拜访（客户名虚构）；周三上午开季度会议；周四上午要做本月业绩简报的准备。请输出为表格：日期、上午、下午、备注。若发现冲突请指出并给出调整建议。`

### 2.3 检查与迭代

完成两项任务后，按以下要点自核并改良：

1. 核对输出：要求是否逐项满足（如任务 B 的栏位是否齐全）？有无捏造内容（虚构情境之外不应出现的数字或名称）？语气是否合适？
2. 选一份输出，**在同一对话追加至少一轮指示改良**。示例：`请把行动事项表加上优先级一栏，并把语气改得更正式。`
3. 记录最有效的一项改良指示。

### 2.4 单元二检查清单

- [ ] 完成两项商务任务，并各检查输出品质
- [ ] 至少完成一轮「追加指示」改良输出
- [ ] 记录一项最有效的改良指示

## 小组讨论题

1. 回顾今日操作：助手在哪些环节展现了「工具使用」「记忆」与「自主执行」？与你用过的网页版聊天工具相比，体验有何不同？
2. 本地 AI 助手（Hermes）与网页版聊天工具各适合什么任务？请各举一例。（提示：档案与系统存取、多步任务 vs 即时问答、跨装置协作）
3. 「在同一对话追加指示」与「重开对话重做」相比，时间与结果有何差异？以今日实测经验说明；这种工作习惯可如何应用到与同事的协作？
4. 助手请求文件权限时你如何判断批准范围？若企业为员工大量部署这类助手，应设哪些使用规则？（提示：最小权限、共用电脑登出、敏感资料界线）

## 常见问题

| 症状 | 处理 |
|---|---|
| 安装卡在 Python 阶段或报版本错误 | 官方曾有此问题（安装器与程式库的 Python 版本锁定不一致，已修正）——重新下载最新安装档再运行；仍失败改用手机热点网络重试 |
| 下载官方网域或 npm 套件超时 | 设代理环境变量后重跑；校园网挡海外则用手机热点 |
| 登录页没有 Google 选项 | 用 GitHub 登录（两者皆可） |
| 见到信用卡输入画面 | 免费方案不需要信用卡——卷到方案列表下方找 **CONNECT** 按钮 |
| 首次启动模型清单空白 | 回 Nous Portal 方案页确认已显示 "CONNECTED"（按 CONNECT）；未解决则重开 Hermes |
| `:free` 模型回应慢或 429 | 免费层限速属正常——稍候片刻重试，或更换其他 `:free` 模型；课堂上各自使用不同模型可分散负载 |
| 选定模型后出现「并非为工具使用设计」类警示 | 该模型未针对工具调用（tool calling）设计——改选其他 `:free` 模型（优先 Nous／Hermes 系列） |
| 1.3 节搬移报错（档案被占用或拒绝存取） | Hermes 正在执行——关闭所有 Hermes 视窗，并于系统匣（工作列右下角）的 Hermes 图示按右键选「结束」；再重跑该三行指令 |
| 搬移后捷径或 `hermes` 指令失效 | 表示原位置连结未建立成功——重跑 1.3 节第三行指令（`New-Item -ItemType Junction ...`）后重试 |
| 以指令启动 Desktop 时首次建构失败（画面大量 npm error） | 属备援路线（以 PowerShell 安装者）之情况，路线 A 不会遇到——① 错误含 `assert-root-install`：于程式库根目录执行 `npm ci` 后重跑（2026-09-05 教师机实测修法）② 错误含 `Access is denied` on `Hermes.exe`：关闭已开启的 Hermes 视窗后重跑 |
| npm 指令报 log 写入失败（指向已不存在的磁碟） | 机器 npm cache 设定指向失效路径——执行 `npm config set cache "C:\Users\<你的用户名>\AppData\Local\npm-cache"` 后重跑（罕见，多见于曾改动磁碟配置的旧机器） |
| 终端机持续出现 simple-git "Invalid value for custom binary"／DEP0180 警告 | 全属无害噪音（上游已知问题 [GitHub issue #79245](https://github.com/NousResearch/hermes-agent/issues/79245)：Windows 路径触发；功能不受影响），忽略即可 |

## 资料与工具速查

| 用途 | 位置 |
|---|---|
| Hermes Desktop 下载与官方文件 | https://hermes-agent.nousresearch.com/desktop ； https://hermes-agent.nousresearch.com/docs |
| Nous Portal（免费方案） | https://portal.nousresearch.com |
| Hermes 本机资料 | `C:\Users\<你的用户名>\Documents\hermes-sandbox\hermes`（设定档 `config.yaml`；安装器原位置 `%LOCALAPPDATA%\hermes` 以连结指向同一资料，仍有效） |
| 工具无法使用时 | 查 AI 工具切换对照指南（已发布于课程 GitHub 仓库 lab/ai-tools-guide.md；Gemini↔Copilot↔WPS AI／DeepSeek 思考模式／harness 说明） |
