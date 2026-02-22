---
title: "Note Attachment Rules（笔记挂载概念规则）"
tags: [system, skos, rules]
date: 2026-02-22
---

# Note Attachment Rules（笔记挂载概念规则）

目的：任何新内容（Inbox/Resources/Atomic/Output）都能明确回答：
- 它属于哪些概念？（可检索）
- 它与哪些旧内容有关？（强关联）
- 它未来如何被复用到模型/输出？（可产出）

---

## 1) 概念挂载的唯一权威方式

统一用 frontmatter 字段 `concepts` 挂载概念卡：

```yaml
---
concepts:
  - "[[concept.<slug>]]"
  - "[[concept.<slug2>]]"
---
```

规则：
- `concepts` 只放“概念卡链接”，不放普通笔记链接。
- 每篇笔记建议 1–5 个概念；超过 7 个通常说明概念过泛或笔记未原子化。

---

## 2) 不同目录的挂载建议

### 00_Inbox
- 允许不完整；但建议至少写 1 个 `concepts`（哪怕是临时概念）。
- Inbox 清理时补全概念挂载并迁移。

### 30_Resources
- `concepts` 用于“素材可被哪些概念调用”。
- 资源笔记正文建议加：**摘录** 与 **我的判断/疑问**（把资源变成 Atomic 的入口）。

### 50_Atomic
- 必须有 `concepts`。
- 每条 Atomic 只表达一个判断；如果一个判断牵涉多个概念，优先拆分。

### 61_Models / 60_Structures
- `concepts` 用于说明该模型覆盖的概念域。
- 模型文件应反向链接到关键 Atomic（证据链）。

### 70_Output
- `concepts` 用于后续复盘：某次输出是从哪些概念/模型组合而来。

---

## 3) 正文“关联区”最小规范

在正文末尾保留一个固定标题：

```markdown
## 关联
- [[相关笔记A]]：为什么相关（一句话）
- [[相关笔记B]]：为什么相关（一句话）
```

这是对“强关联”原则的最低成本实现。

---

## 4) 同义词与自动归档（人工规则）

当你写 Inbox/Atomic 时，用到某个词，如果它是概念卡 `altLabel` 之一：
- 仍然要在 `concepts` 显式挂载该概念卡
- 同时考虑把该词补进对应概念卡的 `altLabel`（如果缺失）

---

## 5) 迁移/重构时的注意事项

- 优先通过 Obsidian 的重命名/移动功能完成（自动更新链接）
- 批量替换前先备份（见 TOOLS_INDEX 维护约定）
