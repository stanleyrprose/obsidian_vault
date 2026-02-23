---
title: "10x-chat（浏览器自动化对话 CLI）"
tags: [resource, tools, browser-automation]
date: 2026-02-23
source: https://github.com/RealMikeChong/10x-chat/blob/main/README-zh.md
---

# 10x-chat（浏览器自动化对话 CLI）

## 解决的问题
- 通过 **Playwright 浏览器自动化**，从终端/编码助手与网页端 AI（ChatGPT、Gemini、Claude、Grok、NotebookLM）对话。
- **只登录一次**，后续 CLI 可复用登录态。
- 支持 `--file` 将文件/Glob 打包成 Markdown 附加到提示里。

## 最短使用流程（Quickstart）
1) 一次性安装浏览器：
```bash
npx playwright install chromium
```

2) 登录（会打开浏览器窗口）：
```bash
npx 10x-chat@latest login chatgpt
# 或 login gemini / claude / grok / notebooklm
```

3) 发起对话（可附文件）：
```bash
npx 10x-chat@latest chat -p "解释这个错误" --provider chatgpt --file "src/**/*.ts"
```

4) 查看历史：
```bash
npx 10x-chat@latest status
```

## 关键能力/参数
- `--file <paths...>`：支持 glob，支持排除 `!pattern`
- `--dry-run`：预览打包内容但不发送（审计/防泄漏）
- `--copy`：仅复制打包内容不发送
- 自动排除敏感文件：`.env*`、`*.pem`、`*.key` 等

## 数据目录结构（排查/迁移用）
```
~/.10x-chat/
├── profiles/<provider>/    # Playwright 持久化浏览器配置（登录态）
├── sessions/<uuid>/        # meta.json / bundle.md / response.md
└── config.json
```

## 适配到我的 OpenClaw 环境（要点）
- VPS 无 GUI：`login` 需要有界面浏览器。
- 推荐在本地电脑执行登录并保存 profile；VPS/agent 侧做“任务拆解 + 文件化产出 + 引用结果”。

## 参考
- 中文 README：https://github.com/RealMikeChong/10x-chat/blob/main/README-zh.md
- Jina Reader：`https://r.jina.ai/<URL>`
