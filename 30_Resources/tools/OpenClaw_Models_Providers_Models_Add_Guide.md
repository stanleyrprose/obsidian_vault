---
title: "OpenClaw：如何添加/允许更多模型（providers & configured）"
tags: [resource, tools, openclaw, models]
date: 2026-02-23
---

# OpenClaw：如何添加/允许更多模型（providers & configured）

> 目的：当你想在 `models.providers.<provider>` 下添加更多 model id，或让 Telegram `/models` 可切换更多模型时，如何安全操作并避免 gateway 崩溃。

---

## 0. 先理解两个概念：Provider 定义 vs “Configured/Allowed”

### A) Provider 定义（`models.providers.<provider>`）
- 描述：某个 provider 的连接方式（baseUrl/apiKey/api）以及该 provider 下有哪些模型（models 列表）。
- 用途：让 OpenClaw 知道怎么调用该 provider。

### B) Configured/Allowed（影响 `/models` 是否 “not allowed”）
- Telegram `/models` 的“可选模型”通常并不是全目录（catalog）都允许，而是只允许：
  - agent 的 primary/fallbacks
  - 或全局/默认的 **configured models** 列表
- 所以：
  - 你可以在 catalog 里“看到”模型，但仍会 `not allowed`
  - 需要把模型加入 configured/allowed 才能切换

**经验结论（本次修正中验证）：**
- 我们之所以能解决 `bailian/* not allowed`，是把 6 个 bailian 模型加入了 **configured models**（通过 `agents.defaults.models` 增加别名/条目），而不是去动 `models.providers`。

---

## 1) 推荐做法（优先级从高到低）

### ✅ 做法 1（最稳）：仅加入“Configured/Allowed”，不要动 provider
适用：
- OpenClaw 已经内置/已配置好了 provider（例如 bailian/minimax/anthropic/openai-codex）
- 你只是想让 `/models` 能切换更多模型

做法：在 `openclaw.json` 中添加：

- `agents.defaults.models["<provider>/<modelId>"] = {"alias":"..."}`

这会让 `openclaw models list` 标记该模型为 `configured`，Telegram `/models` 也更容易允许切换。

示例（bailian）：
```json
"agents": {
  "defaults": {
    "models": {
      "bailian/kimi-k2.5": {"alias": "kimi-k2.5"},
      "bailian/glm-4.7": {"alias": "glm-4.7"}
    }
  }
}
```

> 这是我们这次修复 “not allowed” 的关键。

---

### ⚠️ 做法 2：把模型加入某个 agent 的 fallbacks
适用：
- 只想在某个 agent 下允许切换/或希望模型能自动回退

做法：编辑 `agents.list[].model.fallbacks` 或 `agents.defaults.model.fallbacks`。

注意：
- 这会影响 fallback 链（主模型失败时可能切过去）。
- 仍然可能需要配合“Configured/Allowed”才能在 `/models` 里切换。

---

### ❌ 做法 3（风险最高）：直接编辑 `models.providers.<provider>.models` 增模型
适用：
- provider 完全是你自己新增的
- 或 OpenClaw 目录里根本没有该模型定义

风险：
- provider 定义一旦写错，可能导致 gateway 启动失败（我们在 google/gemini provider 上就遇到过“写入 providers 导致 gateway 不稳定/死机”的情况）。

**结论：除非必要，不建议直接写 providers.models。**

---

## 2) 安全操作流程（必须遵守）

### Step 1：先备份（强制）
对任何 OpenClaw 系统文件修改前：
- 备份格式：`<原文件名>_bak_openAI_<YYYYMMDD>_<HHMMSS>`

示例：
```bash
cp -a /root/.openclaw/openclaw.json /root/.openclaw/openclaw.json_bak_openAI_20260223_123000
```

### Step 2：只改一处，小步迭代
- 一次只加 1~6 个模型 id
- 改完立刻校验 JSON：
```bash
python3 -m json.tool /root/.openclaw/openclaw.json >/dev/null
```

### Step 3：重启 gateway
```bash
systemctl --user restart openclaw-gateway.service
systemctl --user --no-pager status openclaw-gateway.service | head
```

### Step 4：验证模型是否 “configured”
```bash
openclaw models list --all --provider <provider>
openclaw models status | head -n 40
```

### Step 5：Telegram 端验证
在对应 bot 内：
- `/models` → 选择 provider → 选择 model
- 若仍提示 `not allowed`，优先回到“做法 1”把模型加入 configured。

---

## 3) 故障排查（常见坑）

### 1) `not allowed`
- 说明模型不在允许集合（configured/allowed）
- 解决：加到 `agents.defaults.models`（做法 1）

### 2) `missing`
- 说明该 model id 在 catalog/配置中不可用或命名不匹配
- 解决：用 `openclaw models list --all --provider <provider>` 找到真实 id

### 3) gateway 启动失败 / 卡死
- 常见原因：openclaw.json 写坏（非法 JSON）或 provider 配置不兼容
- 解决：立刻回滚备份：
```bash
cp -a /root/.openclaw/openclaw.json_bak_openAI_... /root/.openclaw/openclaw.json
systemctl --user restart openclaw-gateway.service
```

---

## 4) 推荐策略总结

- **想让 `/models` 可选更多模型**：优先改 `agents.defaults.models` 让其成为 `configured`。
- **想新增 provider**：谨慎；先用内置 provider/catelog，除非必要不写 `models.providers`。
- **每次修改必须先备份 + 校验 JSON + 重启 + 验证。**

