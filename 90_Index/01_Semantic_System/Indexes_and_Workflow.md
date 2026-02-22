---
title: "Indexes & Workflow（索引与工作流）"
tags: [system, skos, workflow]
date: 2026-02-22
---

# Indexes & Workflow（索引与工作流）

目标：在不重建目录的前提下，让 SKOS 结构可运行：
- Inbox 不堆积
- 资源能转化为判断
- 判断能进入模型
- 模型能产出
- 产出能复盘并反哺概念体系

---

## 1) 三类索引（建议）

### A. 概念索引（Concept Index）
- 位置：`90_Index/01_Semantic_System/`
- 内容：概念卡规范、概念列表、治理规则

### B. 结构/模型索引（Structures/Models Index）
- 位置：`90_Index/`
- 内容：60/61 的目录化入口（按主题、按用途）

### C. 输出索引（Output Index）
- 位置：`90_Index/`
- 内容：70_Output 的作品集入口 + 与概念/模型的映射

---

## 2) Inbox → 落地的最小流程（每条内容）

当一条新信息进入 `00_Inbox`，按顺序问三件事：

1) **它是什么？**（资源/判断/任务/灵感）
2) **它属于哪些概念？**（写 `concepts`）
3) **它下一步去哪里？**
   - 资源：→ `30_Resources`
   - 单一判断：→ `50_Atomic`
   - 需要组织成体系：→ `61_Models` 或 `60_Structures`
   - 明确要交付：→ `10_Projects` + 产物落 `70_Output`

要求：Inbox 最终必须被清空（≤7 天）。

---

## 3) Atomic → Model → Output 的证据链

- Atomic：写“判断 + 条件 + 反例 + 引用来源”
- Model：把多个 Atomic 组织成可复用结构（含适用边界）
- Output：在产出中引用 Model/Atomic，并在复盘中验证

强制建议（可渐进执行）：
- 每篇 Model 至少链接 3 条 Atomic
- 每篇 Output 至少链接 1 个 Model（或说明为什么不用）

---

## 4) 概念治理（每周一次，15–30 分钟）

- 新增概念：是否已有同义概念？先搜 `altLabel`
- 合并概念：标记 deprecated + deprecatedBy（见概念卡规范）
- 过泛概念：拆分 narrower
- 关系校正：related vs broader/narrower 不混用

---

## 5) 下一步：建立 Concepts 目录（如果尚未存在）

如果你的 vault 里还没有概念卡的物理目录，建议创建：
- `90_Index/01_Semantic_System/Concepts/`

然后从最高频的 20–50 个概念开始补卡。
