---
type: lab
tags:
  - teaching
  - msba6125
  - lab
  - guide
created: 2026-08-19
updated: 2026-09-24
---

# 第 4 课｜Colab 学术检索实作：Scopus 文献搜寻与开放取用 PDF 下载

> 本单元动手实作：于 Elsevier Developer Portal 申请个人 Scopus API key → 以 Colab 云端笔记本串接 Scopus 学术资料库完成两阶段文献检索（宽泛检索 → 标题筛选 → 聚焦检索）→ 查询各篇之开放取用（Open Access, OA）状态并下载可合法取得之 PDF，无法开放取用者保留书目资讯。
>
> 完成检索后进行对照练习：向 AI 聊天机器人索取同一主题之文献，逐笔核对其引用是否真实存在于文献资料库，统计真实比例。本实作对应课程主题「资讯系统中的伦理与社会议题」，并训练「检索 → 筛选 → 合法取得 → 对照验证 → 人工核对」完整工作流。
>
> **范围说明**：本 Lab 仅示范以 API 连接 Scopus 取得论文书目资讯（含摘要撷取）——即研究流程「资料取得」环节之机制理解；检索结果之研究缺口（research gap）分析属代理式 AI（agentic AI）之应用（后续课程主题），非本 Lab 范围。

## 学习目标（Learning Objectives）

完成本单元后，你将具备：

- 于 Elsevier Developer Portal 申请个人 Scopus API key，并以 Colab Secrets 安全存放（不写入程式码）
- 于 Colab 执行 Python 笔记本：执行单元、读取输出表格、下载档案
- 以 Colab 内建 Gemini 助理定位程式错误并修正（除错实务）
- 以 Scopus Search API 完成两阶段文献检索（宽泛 → 聚焦，含摘要撷取），并比较两轮结果之差异
- 以 Unpaywall API 查询论文之开放取用（OA）状态，下载开放取用之 PDF；非 OA 论文仅保留书目资讯（metadata）
- 以 AI 聊天机器人回答同一文献检索问题，逐笔核对其引用与文献资料库实际纪录是否一致，记录核对结果
- 说明学术资源之存取授权差异与合法下载界线，并以 Scopus 网页完成人工核对
- 说明 AI 辅助除错之正确用法：AI 定位与建议、学生验证与理解修正内容

## 课前准备（Pre-class Requirement）

- 已登入 Google 帐号且可执行 Colab（第 1 课已完成环境部署；未完成者先依 Lab_Ch01 补做）
- 学校邮箱（申请 Elsevier API key 用）
- 一台可连网之电脑（课堂或自备）
- 已初步选定一个与管理或资讯系统相关之检索主题（示例：digital transformation in SMEs、AI in marketing、customer churn prediction；课堂上可调整）
- 无法使用 Colab 的同学：替代环境见 `ai-tools-guide.md`「Colab 的中国替代」节（魔搭 ModelScope Notebook，可直接上传本课 .ipynb 档案运行）

## 操作步骤

以下步骤以各平台官方手册为准（以 2026-09 为准；平台规则可能调整，如遇变动以官方最新说明为准）。

### 步骤 1：申请 Scopus API key（建议课前完成）

- 官方入口：https://dev.elsevier.com （Elsevier Developer Portal）
- 操作：
  1. 浏览器开启上述网址；未注册者先注册帐户——建议使用学校邮箱，机构栏填 Macao Polytechnic University；已有帐户者直接登录
  2. 按页面「I want an API Key」按钮（或直接开启 https://dev.elsevier.com/apikey/manage ）
  3. 依表单指示填写：产品（product）选 Scopus Search API；用途选学术研究／非商业
  4. 提交后，页面显示一串字母数字组合之 API key（形如 `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`）——复制保存，步骤 4 使用
- 预期结果：My API Key 页面显示你的个人 key（日后可返回此页查阅）
- 说明：任何人皆可申请 API key；Scopus 之完整存取依所属机构对 Elsevier 产品之订阅授权——以学校邮箱注册可得较完整之栏位与配额
- 失败处理：表单栏位以页面当日显示为准；找不到按钮时，先确认已登入

### 步骤 2：开启本课笔记本（GitHub → Colab，或 Canvas 下载后上传）

本步骤提供两种方式——择一即可：

**方式 A：经课程 GitHub 仓库**
- 官方入口：https://colab.research.google.com/github/aiccuser1/msba6125-2026/blob/main/lab/ch04/Lab_Ch04.ipynb （本课 notebook）
- 操作：
  1. 浏览器确认已登入 Google 帐号（未登入时 Colab 会要求先登入——必须登入才能开启与执行）
  2. 开启上述连结——Colab 直接载入课程仓库中之 notebook

**方式 B：自 Canvas 下载后上传至 Colab**
- 官方入口：https://colab.research.google.com （Colab 首页）
- 操作：
  1. 于 Canvas 课程页下载本课 notebook 档（`Lab_Ch04.ipynb`）
  2. 浏览器开启 Colab 首页并登入 Google 帐号
  3. 于首页「Upload」页签选择下载之 `Lab_Ch04.ipynb`（或于已开启笔记本之选单 File（档案）→ Upload notebook（上传笔记本）操作）
  4. notebook 于新分页开启

**两种方式接续之共同操作**：选单 File（档案）→ Save a copy in Drive（在云端硬碟中另存副本）——之后一律在你的个人副本上操作

- 预期结果：Colab 开启你的个人副本（标题含「Copy of」）
- 说明：notebook 原版位于课程仓库 `lab/ch04/Lab_Ch04.ipynb`（Canvas 亦提供同一档案之下载）；个人副本保存于你的 Google Drive，原版不会被更动
- 失败处理：Colab 显示登入画面 → 先完成 Google 帐号登入再重试；无法存取 Colab（网路受限）→ 改用替代环境（见课前准备最后一项）；无法存取 GitHub（网路受限）→ 改用方式 B

### 步骤 3：以 Colab 内建 Gemini 除错（AI 辅助除错实作）

本课 notebook 含一处程式错误——**这是刻意设计的除错练习**。你将使用 Colab 内建的 Gemini 助理找出错误并修正，体验「AI 辅助除错」之正确用法：AI 负责定位与建议，你负责验证与理解每一处修正。

- 操作：
  1. 接续步骤 2——于步骤 2 完成开启之个人副本中，依序执行单元至出现错误讯息者（该单元左侧显示红色叉号）
  2. 于 Colab 版面右侧或左侧边栏点击 **Gemini 图示**（星形／Gemini 标志）开启 AI 助理面板
  3. 将错误讯息贴入对话框，并描述情境——示例：`执行这个单元时出现以下错误：<贴上错误讯息>。请找出原因并提供修正方式。`
  4. 阅读 Gemini 之诊断与修正建议——**先理解错在哪里**，再依建议修改：将修正后之程式码贴回原单元，或直接采用面板提供之差异检视（diff view）套用修正
  5. 重新执行该单元直至通过（单元左侧显示绿色勾号）
- 预期结果：除错后之单元执行成功——**此为全部后续步骤之执行前提**
- 记录：以一句话写出错误原因与修正方式（例如：「变数名称拼写不一致——修正为函式定义之名称」）
- 说明：**VPN 提醒**——Gemini 于部分地区需经 VPN 方可使用；于 Colab 内使用 Gemini 助理同受此限，无法连线时先开启 VPN 再重试。Colab 之 Gemini 助理以官方公告为准（见下方官方指引）；AI 除错之产出同样须经你自行验证——对修正内容有疑问时追问 Gemini「为什么」，直至理解为止
- 失败处理：Gemini 面板无法连线或无回应 → 开启 VPN 后重试；找不到 Gemini 图示 → 确认已登入 Google 帐号后重新载入页面；除错后仍失败 → 将新错误讯息再次贴入 Gemini 面板，追问直至解决

### 步骤 4：将金钥存入 Colab Secrets

- 官方指引：https://colab.research.google.com/notebooks/secrets.ipynb （Colab 官方 Secrets 说明）
- 操作：
  1. 于 Colab 视窗左侧边栏点击钥匙图示（Secrets）开启面板
  2. 点 Add new secret，依下述新增两个项目（每项填写后开启 Notebook access 切换钮）：
     - Name：`SCOPUS_API_KEY`；Value：步骤 1 取得之 key
     - Name：`CONTACT_EMAIL`；Value：你的学校邮箱（Unpaywall 查询要求真实邮箱）
  3. 回到笔记本，执行第一个单元「环境设定」——执行方式：点击单元左侧 ▶ 播放按钮（或选中单元后按 Shift+Enter）；执行期间单元左侧显示转圈，完成后显示绿色勾号
- 预期结果：单元输出「金钥已载入」讯息（仅显示成功讯息，不显示金钥内容）
- 说明：Secrets 储存于你的 Google 帐号、加密保存，不会写入笔记本档案，分享笔记本时亦不外泄；金钥仅供个人使用——勿贴入程式码、勿传给他人
- 失败处理：单元回报找不到 `SCOPUS_API_KEY` → 检查名称拼写（不可含空格）与 Notebook access 切换钮是否已开启；其他错误依单元输出之提示处理

### 步骤 5：第一轮检索（宽泛）

- 操作：
  1. 向下卷动至「第一轮检索」单元，找到 `<你的主题>`，整体取代为你的检索主题文字
  2. 执行该单元——笔记本以 Scopus Search API 检索，输出前 10 笔结果（标题、年份、DOI、被引次数、期刊与摘要）
  3. 阅读标题与摘要，选出与你主题最相关的 5 篇
- 预期结果：10 笔检索结果，每笔含书目资讯与摘要（摘要于画面显示前 200 字元，全文存于汇出 CSV）
- 说明：查询语法 `TITLE-ABS-KEY(...)` 同时检索标题、摘要与关键词栏位；`PUBYEAR > 2022` 限 2023 年以来之文献。摘要经 COMPLETE 检视取得；个别论文 Scopus 未收录摘要时显示「（无摘要）」
- 失败处理：0 笔结果 → 主题词过窄——减少关键词或改用更广泛之同义词后重跑此单元；显示 401／未授权 → 回步骤 1、4 检查 key；显示配额（quota）提示 → 稍候数分钟再试

### 步骤 6：第二轮检索（聚焦）

- 操作：
  1. 从步骤 5 所选 5 篇之标题与摘要归纳 1–2 组更精确之关键词组合
  2. 在「聚焦检索」单元改写查询后执行——示例：`TITLE-ABS-KEY(("generative AI" OR "large language model") AND (advertising OR "consumer engagement"))`
  3. 比较两轮结果之差异（结果数量、主题集中度）
- 预期结果：第二轮结果数量减少、主题更聚焦于你关心的子议题
- 记录：以一句话写出「窄化后聚焦于哪个子议题」
- 说明：两阶段检索为研究工作之标准起手式——宽泛检索掌握全局，聚焦检索逼近研究问题
- 失败处理：第二轮 0 笔 → 放宽至单一组关键词再试

### 步骤 7：开放取用（OA）查询与 PDF 下载

- 官方指引：https://unpaywall.org/products/api （Unpaywall API 文件；免注册）
- 操作：
  1. 执行「开放取用查询」单元——笔记本以各篇之 DOI 逐一查询 Unpaywall，输出 OA 状态表
  2. 执行「下载开放取用 PDF」单元——可开放取用之论文下载至档案区：点 Colab 左侧边栏资料夹图示（Files）查看，档案位于 `downloads` 资料夹
- 预期结果：每篇显示 OA 状态；OA 论文下载成功（档名含 DOI）；非 OA 论文显示「仅保留书目资讯」
- 记录：统计可下载篇数与非 OA 篇数
- 说明：本练习仅下载开放取用版本（作者／出版社授权公开之版本）；订阅制论文之全文取得须经机构订阅或图书馆服务——自动下载仅限合法授权范围，此即真实研究之常态
- 失败处理：查询显示 422 错误 → `CONTACT_EMAIL` 须为真实格式之邮箱（回步骤 4 修正）；个别论文下载失败 → 跳过并记录，不影响其余篇数

### 步骤 8：对照练习——AI 聊天机器人引用核对

- 操作：
  1. 于浏览器新分页开启你的 AI 聊天机器人（课程帐号：Gemini；未使用 Google 帐号者可用本课常用之国产模型，如 DeepSeek、Kimi）
  2. 以下列提示词提问，要求 3 篇附完整引用资讯（标题／年份／DOI）之文献：
     - 提示词范例：`请推荐 3 篇 2023 年以后与「<你的主题>」相关的学术论文，并提供每篇的标题、年份、DOI`
  3. 在 notebook「AI 对照核对」单元填入聊天机器人回复之标题、年份、DOI（依单元内注解之格式）
  4. 执行该单元——notebook 以你先前检索取得之文献资料库纪录，逐笔核对聊天机器人提供之 DOI
- 预期结果：核对表格逐笔显示「已核实」（资料库有此文）／「资料不符」（有此 DOI 但书目不同）／「查无此文」（可能存在虚构引用）；单元最后显示真实核对率统计
- 记录：聊天机器人提供 3 篇中，已核实几篇、资料不符几篇、查无此文几篇——以统计结果写出一句结论（例如：「3 篇中 2 篇经核实，1 篇查无此文」）
- 说明：大型语言模型（LLM, Large Language Model）可能产生看似真实、实际不存在的「虚构引用（hallucinated citation）」；AI 生成之引用资讯一律须经资料库核实，不得直接采用——此即本单元「AI 产出、人工把关」之核心训练
- 失败处理：3 篇皆查无此文 → 换一组提示词或另一聊天机器人再试一次；部分 DOI 为空 → 以标题搜寻 Scopus 网页核对

### 步骤 9：人工核对与结果存档

- 操作：
  1. 执行「汇出结果」单元——检索结果存为 CSV 档，于档案区下载保存
  2. 浏览器开启 Scopus 网页 https://www.scopus.com
  3. 以其中 1 篇之标题搜寻，核对该篇之作者、年份与 DOI 是否与笔记本输出一致
- 预期结果：核对一致；CSV 已下载（档名形如 `scopus_results_<你的主题>.csv`）
- 说明：程式与 AI 代劳之后，人工核对为最后防线（课程一贯原则）；核对以校园网络内进行为佳
- 失败处理：Scopus 网页显示存取受限 → 于校园网络内重试；搜寻不到该篇 → 改用完整 DOI 搜寻核对

## 完成标准（Deliverables）

- [ ] Elsevier API key 申请成功
- [ ] 除错练习完成：程式错误经 Colab Gemini 定位并修正，错误单元执行通过＋一句原因记录
- [ ] Colab 个人副本开启，「环境设定」单元显示金钥已载入
- [ ] 第一轮检索完成（10 笔结果，含摘要），并选出 5 篇
- [ ] 第二轮聚焦检索完成，并以一句话记录窄化后之子议题
- [ ] OA 状态表完成（记录：可下载篇数／非 OA 篇数）
- [ ] 至少 1 篇 OA PDF 已下载至档案区（若所选皆非 OA：放宽主题重跑一轮，直至取得至少 1 篇）
- [ ] 1 篇经 Scopus 网页人工核对（作者／年份／DOI 一致）
- [ ] 对照记录：聊天机器人 3 篇引用之核对结果（已核实／资料不符／查无此文篇数）＋一句结论
- [ ] 检索结果 CSV 已存档

## 工具对照

工具无法使用时，查 AI 工具切换对照指南（已发布 GitHub：aiccuser1/msba6125-2026 lab/ai-tools-guide.md；含 Colab 之替代环境「魔搭 ModelScope Notebook」与各工具说明）。
