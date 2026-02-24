---
title: "KB 查询标准流程（tree + qmd 省 token）"
tags: [system, playbook, kb, qmd]
date: 2026-02-24
---

# KB 查询标准流程（tree + qmd 省 token）

目标：在 SKOS vault 中查找信息/生成输出时，尽量减少模型上下文与重复粘贴，从而降低 token 消耗。

本流程采用：
- **tree**：生成 vault 目录树索引（替代“MDtree”）
- **qmd**：对 markdown 内容做本地检索与按需展开

---

## 0) 生成/更新目录树索引（Index）

索引文件：
- `90_Index/vault_tree.txt`

更新命令（VPS）：
```bash
apt-get install -y tree

tree -a -L 4 /root/obsidian_vault > /root/obsidian_vault/90_Index/vault_tree.txt
```

建议：
- 每天 1 次即可（或每次大改目录后手动更新）

---

## 1) 触发场景（何时自动用本流程）

当用户问题满足任一条件时，优先走“tree + qmd”：
- “在 KB/vault 里找…”、“我之前记过…”、“哪份文档提到过…”
- 需要跨多个笔记/项目汇总
- 输出型任务（70_Output）需要引用多篇 Atomic/Models

---

## 2) 查询流程（Query）

### Step A：先看 tree（确定范围）
- 通过 `vault_tree.txt` 快速定位可能的目录（Projects/Areas/Resources/Atomic/Models 等）

### Step B：用 qmd 做检索（缩小到 TopN 文档）

推荐命令：
- 关键词搜索：
```bash
qmd search "关键词" -c workspace
```

- 深度搜索（混合 + rerank）：
```bash
qmd query "你的问题" -c workspace
```

### Step C：按需展开（只读必要文档）

- 拉取某篇文档：
```bash
qmd get "qmd://workspace/path/to/file.md"
```

原则：
- 优先只读 Top 3–8 篇
- 每篇只截取必要段落（避免全文灌入模型）

---

## 3) 输出流程（Write）

当需要形成新产出时：
- 结论/输出写入对应目录（例如 `70_Output/...`）
- 同时在输出文档末尾记录引用来源路径（便于追溯）
- 聊天中只回复：
  - 1–5 句摘要
  - 文件路径（或 GitHub 链接）

---

## 4) 约定

- KB 相关任务需先阅读并遵循：`90_Index/00_SYSTEM_RULES.md`
- 工具索引：`/root/.openclaw/workspace/TOOLS_INDEX.md`
