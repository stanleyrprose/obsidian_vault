---
title: "Concept Card Spec（概念卡最小规范）"
tags: [system, skos, concept]
date: 2026-02-22
---

# Concept Card Spec（概念卡最小规范）

目的：把“SKOS 概念”落到 Obsidian 文件层，确保：
- 可维护（合并/改名不破坏）
- 可检索（同义词、定义、关系）
- 可挂载内容（Atomic/Resources/Output 都能指向概念）

> 注意：这里的“概念卡”是**索引层对象**，不是长文。长内容应该落在 Atomic/Models/Resources/Output。

---

## 1) 概念卡文件位置

推荐路径（统一收敛到 Index，便于治理）：
- `90_Index/01_Semantic_System/Concepts/<slug>.md`

如现有 vault 已有 Concepts/Taxonomy 目录，可保持原位；但需满足下述字段与命名规则。

---

## 2) 文件命名

- 使用稳定 slug：`concept.<slug>.md` 或 `<slug>.md` 二选一，团队内保持一致。
- slug 建议：小写英文 + `-`，避免中文改名导致链接脆弱。
- 展示名称用 `prefLabel`（支持中文）。

---

## 3) 概念卡最小字段（YAML Frontmatter）

每张概念卡至少包含：

```yaml
---
type: skos_concept
prefLabel: "<首选标签｜中文/英文均可>"
altLabel:
  - "<同义词1>"
  - "<同义词2>"
definition: "<一句话定义：该概念是什么>"
scopeNote: "<使用范围/边界：什么时候用，什么时候不用>"
broader:
  - "[[<上位概念卡>]]"
narrower:
  - "[[<下位概念卡>]]"
related:
  - "[[<关联概念卡>]]"
examples:
  - "<例子1>"
  - "<例子2>"
source:
  - "<来源：书/文章/经验>"
created: 2026-02-22
updated: 2026-02-22
---
```

说明：
- `broader/narrower/related` 用 Obsidian wiki link，保持可点击。
- `altLabel` 是检索和归档的关键：写笔记时出现同义词也能落到同概念。
- `definition/scopeNote` 追求“可操作”的边界，不要百科式堆字。

可选字段（需要时再加）：
- `exactMatch/closeMatch`: 外部本体/维基/标准词表链接
- `status`: draft/stable/deprecated
- `deprecatedBy`: 合并后指向新概念

---

## 4) 正文结构（推荐）

概念卡正文保持短、结构化：

- **核心定义**：补充 3–5 句话（可选）
- **判别条件**：
  - 属于该概念的必要条件
  - 常见误用/反例
- **关键链接**：
  - 指向 50_Atomic 中最关键的原子判断（最多 5 条）
  - 指向 61_Models/70_Output 中最相关的成果（最多 5 条）

---

## 5) 合并/改名规则（不破链）

- 尽量不移动概念卡文件位置；需要移动时，使用 Obsidian 重命名以保留链接更新。
- 概念合并：
  1) 将旧概念 `status: deprecated`
  2) 增加 `deprecatedBy: "[[新概念]]"`
  3) 在旧概念正文第一行写清“已合并到…”
  4) 分批把引用旧概念的笔记迁移到新概念

---

## 6) 最小可行（MVP）

如果你现在概念很多、来不及补全字段：
- 先保证每张概念卡至少有：`type + prefLabel + broader(若有) + definition(一句话)`
- `altLabel/related/scopeNote` 逐步补
