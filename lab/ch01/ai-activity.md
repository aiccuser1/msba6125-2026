---
type: guide
tags:
  - teaching
  - msba6125
  - ai-activity
  - guide
created: 2026-08-19
updated: 2026-08-19
---

# 第 1 课｜课程公告与实验环境准备

> 本课无实作活动。课程自第 2 课起每周设 AI 实作环节。本节课讲解并示范实验环境建置，请按本指引课后在家完成个人环境准备。


## 课程 AI 实作安排

- 自第 2 课起，每节课末设 AI 实作环节；未完成部分课后自行完成
- 内容连续进度：导论 → 环境建置 → 各工具应用（文书／Excel／Email／agent）→ 课程结束延伸
- 教材交付：活动指引 PDF 由教师上传 Canvas；实验代码（notebook）发布于 GitHub 课程仓库（github.com/aiccuser1/msba6125-2026），学生下载至自己的 Colab 使用（每人自留完整副本）

## 设备准备（BYOD）

- 自第 2 课起，每组（5-6 人、每课临时分组）至少 1 台笔记本电脑，组内轮流操作
- 未携带笔记本电脑者：网页端活动可用手机完成（备用方案）
- 建议课前完成下文环境建置，避免课内等待

## 实验环境建置（课后在家完成）

以下六步依据各平台官方手册编写（以 2026-08 为准；平台规则会调整，如遇变动以官方最新说明为准）。

### 步骤 1：注册 Google 账号

- 用途：Gemini 网页版与 Colab 均需 Google 账号登录
- 官方指引：support.google.com/accounts/answer/27441（Google 账号帮助中心）
- 操作：访问 accounts.google.com/signup → 填写姓名、用户名（作为邮箱地址）、密码 → 按提示完成验证
- 已有 Google 账号者跳过此步

### 步骤 2：获取 Gemini API 密钥（API key）

- 用途：第 5 课起 Colab 编程调用 Gemini 模型；第 2-4 课网页版对话无需密钥
- 官方指引：ai.google.dev/gemini-api/docs/api-key（Gemini API 官方文档「API keys」）
- 操作：访问 aistudio.google.com/apikey → 登录 Google 账号 → 点击「Create API key」→ 复制并妥善保存
- 说明：免费层无需绑定信用卡；密钥仅限个人使用，不得外传（第 8 课安全实务详解数据边界）；部分账号处于「authorized key」过渡期，需按官方提示完成开发者验证

### 步骤 3：Colab 首次运行

- 用途：云端 Python 笔记（notebook）运行环境；第 5 课起全部实验在此完成
- 官方入口：colab.research.google.com
- 操作：访问上述网址 → 登录 Google 账号 → 打开任意示例 notebook → 依次点击各代码单元（cell）左侧的播放按钮运行 → 确认能正常输出
- 说明：免费层资源与用量有限制；本课程每课请求量小，免费层足够

### 步骤 4：注册 GitHub 账号

- 用途：课程实验代码（notebook）经 GitHub 发布与获取
- 官方指引：docs.github.com（GitHub 文档「Creating an account on GitHub」）
- 操作：访问 github.com/signup → 填写邮箱、密码、用户名 → 完成邮箱验证
- 说明：下载公开仓库无需登录；建议注册以便保存个人副本

### 步骤 5：试运行「GitHub → Colab」链路

- 用途：验证课程实验代码的获取流程；第 5 课起每课沿用
- 操作：
  1. 打开课程仓库：github.com/aiccuser1/msba6125-2026
  2. 找到实验 notebook 文件，点击「Open in Colab」按钮（或复制网址后按格式 colab.research.google.com/github/<仓库路径> 访问）
  3. Colab 打开后：菜单 File（文件）→ Save a copy in Drive（在云端硬盘中另存副本）
  4. 在个人副本中依次运行各单元，确认环境可执行
- 说明：始终操作个人副本，不修改原文件；每课实验完成后保存本人版本

### 步骤 6：安装 ima 桌面版

- 用途：知识库问答工具（NotebookLM 在澳门与中国无法存取，ima 为替代方案）
- 官方入口：ima.qq.com（腾讯 AI 智能工作台；支持 Windows／macOS／iOS／Android）
- 操作：访问 ima.qq.com → 下载对应系统桌面版 → 安装 → 手机号或微信登录 → 新建知识库 → 上传课程讲义（PDF／Word）→ 基于文档提问，答案附原文出处
- 说明：免费版提供基础存储空间（以官方最新说明为准）；支持 PDF、Word、PPT、Excel 等格式
- 中国同学补充：如 Google 生态部分工具不可用，可同时注册 DeepSeek／Kimi 等替代工具（用法见「工具对照」节），本课程活动均提供对应用法

## 课后建置清单

- [ ] 注册 Google 账号（或确认已有）
- [ ] 获取 Gemini API 密钥并妥善保存
- [ ] Colab 首次运行成功
- [ ] 注册 GitHub 账号并完成邮箱验证
- [ ] 从课程仓库成功打开 notebook → Save a copy → 运行成功
- [ ] （中国同学）安装 ima 桌面版并上传一份讲义试用
- [ ] 自第 2 课起携带笔记本电脑（每组至少 1 台）

本课以建置清单完成为完成标准（教师抽查），无书面交付。

## 工具对照

工具无法使用时，查 AI 工具切换对照指南（Gemini↔Copilot↔WPS AI／DeepSeek 思考模式／harness 说明／ima 知识库）。
