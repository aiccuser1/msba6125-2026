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

# 第 1 课｜实验环境部署与提示词基础

> 本单元包含两项核心学习任务：（1）依六步流程完成个人实验环境之部署，须于下次课前完成；（2）习得提示词（prompt）之基础概念，并借由对话式 AI 代理（conversational AI agent）完成一篇指定课程文献之翻译——该任务旨在融合文本阅读与自主探究式学习。

## 学习目标（Learning Objectives）

完成本单元后，你将具备：

- 部署个人实验环境之能力（Google 帐号、Gemini API 金钥、Colab、GitHub 帐号、NotebookLM）
- 提示词三要素（明确性 specific／角色 role／格式 format）之理解与应用
- 以「AI 翻译＋人工校对」工作流完成英文文献阅读之方法

## 课前准备（Pre-class Requirement）

- 一台可连网之电脑或手机
- 一个有效信箱（用于帐号注册）

## 第一部分：实验环境部署（六步协议，课前完成）

以下六步依据各平台官方最新手册编写（以 2026-08 为准；平台规则可能调整，如遇变动以官方最新说明为准）。

### 步骤 1：注册 Google 帐号

- 用途：Gemini 网页版、Colab 与 NotebookLM 均需 Google 帐号登入
- 官方指引：support.google.com/accounts/answer/27441（Google 帐号说明中心）
- 操作：访问 accounts.google.com/signup → 填写姓名、使用者名称（作为信箱）、密码 → 依提示完成验证
- 已有 Google 帐号者跳过此步

### 步骤 2：取得 Gemini API 金钥（API key）

- 用途：后续课程以 Colab 程式呼叫 Gemini 模型；网页版对话无需金钥
- 官方指引：ai.google.dev/gemini-api/docs/api-key（Gemini API 官方文件「API keys」）
- 操作：访问 aistudio.google.com/apikey → 以 Google 帐号登入 → 点选「Create API key」→ 复制并妥善保存
- 说明：免费层无需绑定信用卡；金钥仅限个人使用，不得外传

### 步骤 3：Colab 首次执行

- 用途：云端 Python 笔记（notebook）执行环境
- 官方入口：colab.research.google.com
- 操作：访问上述网址 → 以 Google 帐号登入 → 开启任一范例 notebook → 依序点击各程式单元（cell）左侧执行钮 → 确认输出正常
- 说明：免费层资源与用量有限制；本课程每课请求量小，免费层足够

### 步骤 4：注册 GitHub 帐号

- 用途：课程实验程式（notebook）经 GitHub 发布与取得
- 官方指引：docs.github.com（GitHub 文件「Creating an account on GitHub」）
- 操作：访问 github.com/signup → 填写信箱、密码、使用者名称 → 完成信箱验证
- 说明：下载公开仓库无需登入；建议注册以便保存个人副本

### 步骤 5：验证「GitHub → Colab」链路

- 用途：验证课程实验程式之取得流程
- 操作：
  1. 确认浏览器已登入你的 Google 账号（未登入时 Colab 会要求先登入——必须登入才能开启与执行）
  2. 于浏览器开启以下网址（Colab 直接加载 GitHub 仓库中的 notebook）：
     https://colab.research.google.com/github/aiccuser1/msba6125-2026/blob/main/lab/ch01/Lab_Ch01.ipynb
  3. Colab 开启后（以你的账号工作阶段载入）：选单 File（档案）→ Save a copy in Drive（在云端硬碟另存副本——否则只是临时载入，不会保存）
  4. 于个人副本中依序执行各单元（环境验证＋基本运算＋Gemini API 连线测试），确认输出显示 OK
- 说明：一律操作个人副本，不修改原档；API 金钥以 Colab Secrets 储存（notebook 内含设定说明）

### 步骤 6：认识 NotebookLM（Gemini Notebook）

- 用途：知识库问答工具——以课程文件为基础提问，答案附原文出处；本课程介绍此工具，你可自行选用
- 说明：NotebookLM 已更名为 **Gemini Notebook**（介面与功能相同，官方新名称）；登入 notebooklm.google.com 后即可使用
- 操作：访问 notebooklm.google.com → 以 Google 帐号登入 → 新建笔记本 → 上传课程讲义（PDF／Word）→ 基于文件提问，答案附原文出处
- 无法存取 Google 服务者：可使用替代工具（见文末「工具对照表」——如元宝＋ima 桌面版），用法与 NotebookLM 相同

## 第二部分：提示词基础（三要素）

提示词（prompt）为你对 AI 之指令。有效提示词包含三项要素：

| 要素 | 定义 | 反例 → 正例 |
|---|---|---|
| 明确性（specific） | 明定任务对象、范围与约束 | 「帮我翻译这段」→「将下列英文段落翻译为简体中文，术语保留英文原文并于括号内注明」 |
| 角色（role） | 指定 AI 之身份／视角 | 「翻译一下」→「你是一位专业译员，擅长管理资讯系统领域之中英互译」 |
| 格式（format） | 指定输出之组织方式 | 「总结一下」→「以三个要点条列，每点不超过 50 字」 |

> 原则：同一任务，提示词越具体，输出越可用。此为后续各单元之核心技能。

## 第三部分：应用实践——以 AI 翻译完成指定阅读

课程于 Canvas 提供每章阅读材料（英文原文）。本单元藉对话式 AI 代理完成阅读：

1. 开启 Canvas 课程页，下载 **Reading_Ch01** 任一篇文章（英文原文）
2. 开启 Gemini 网页版（gemini.google.com）或其他对话式 AI 代理（元宝／DeepSeek／Kimi 等）
3. 复制文章第一段，输入以下提示词：

```
你是一位专业译员，擅长管理资讯系统领域的中英互译。
请将以下英文段落翻译为简体中文：
- 术语首次出现时保留英文原文并于括号内注明
- 保持学术语气
- 直译优先，不意译

[贴上文章段落]
```

4. 对照译文与原文，检核：术语是否保留？语句是否通顺？有疑问处再次提问（如「此句之 out-of-bounds read 为何意？」）
5. 重复步骤 3-4，完成整篇文章之翻译阅读

**完成标准**：能以自身语言向同学复述该文要点（不要求缴交译稿——翻译为阅读之手段，非交付物）。

## 课后完成清单

- [ ] 注册 Google 帐号（或确认已有）
- [ ] 取得 Gemini API 金钥并妥善保存
- [ ] Colab 首次执行成功
- [ ] 注册 GitHub 帐号并完成信箱验证
- [ ] 自课程仓库 `lab/ch01/` 成功开启 notebook → Save a copy → 执行成功
- [ ] 启用 NotebookLM（Gemini Notebook）并上传一份讲义试用
- [ ] 以对话式 AI 代理完成至少一段 Reading_Ch01 文章之翻译阅读

## 工具对照表

本课程以 Google 生态（Gemini＋Colab＋NotebookLM）为主。任一工具无法使用时（如网路环境受限），按下表切换替代工具：

| 用途 | 主力工具 | 替代工具 |
|---|---|---|
| 对话／翻译 | Gemini（gemini.google.com） | 元宝（yuanbao）／DeepSeek／Kimi／WPS AI |
| 知识库问答 | NotebookLM（Gemini Notebook） | 元宝＋ima 桌面版 |
| 程式执行 | Colab | 本地 Python 环境 |
| 文书处理 | Google Docs | Word＋WPS AI |

各平台免费额度与规则会调整，以官方最新说明为准。本课程练习请求量小（每课数条 prompt），免费档足够。

> 替代工具之详细 prompt 对照与 DeepSeek 思考模式说明，见课程 GitHub 仓库 `lab/ai-tools-guide.md`（可选阅读）。
