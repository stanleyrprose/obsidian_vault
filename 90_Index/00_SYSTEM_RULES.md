---
title: "系统规则（SKOS 全局约定）"
tags: [system, skos, rules]
date: 2026-02-22
---

# 系统规则（SKOS 全局约定）

> 适用范围：所有 agent 参与“个人知识库 / 第二大脑 / Obsidian vault”相关任务时，都必须遵循本规则。

## 1) 目录结构（必须遵循）

SKOS v1.0 顶层目录：

- `00_Inbox`：临时捕获区（不超过 7 天，定期清空）
- `10_Projects`：有截止日期的项目
- `20_Areas`：长期责任区
- `30_Resources`：素材仓库（不是思考区）
- `40_Archive`：归档
- `50_Atomic`：原子笔记（每条只表达一个判断）
- `60_Structures`：跨领域结构模型库
- `61_Models`：系统模型/决策框架
- `70_Output`：输出（文章/决策/PPT）
- `80_Hypotheses`：假设库
- `81_Decision_Trees`：决策树
- `82_Postmortem`：复盘
- `90_Index`：索引
- `91_Dashboard`：仪表盘

权威说明文件：`/root/obsidian_vault/README.md`（GitHub 同步版）。

## 2) 写入原则

- **不在聊天里粘贴长内容**：长清单/长摘录写入 vault 文件，并只回复“摘要 + 文件路径”。
- **原子化**：能拆就拆，每条 Atomic 只讲一个判断。
- **强关联**：新笔记要尽量链接到旧笔记（或在“关联”区写出关联标题）。

## 3) 文件化工作流（省 token）

长任务必须建立项目目录，并使用模板：
- 模板：`/root/.openclaw/workspace/projects/_template/`
- brief.md：目标/约束/交付物
- working.md：候选清单/证据/状态

## 4) 模型使用规则（省 token）

- **L1/L2**：优先 minimax（能用工具就不用模型）
- **L3**：涉及代码/排障/改配置才用 codex

详见：`90_Index/Model_Usage_Rules.md`

## 5) 产出约定

- 任何“新增/整理”的内容必须落盘到 vault 或 workspace 文件中
- 回复用户时给：
  - 1–5 句摘要
  - 文件路径（或 GitHub 链接）
  - 下一步建议
