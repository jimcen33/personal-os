# 添加一个 Workstation

什么时候需要扩展系统、加一个现有 workstation 覆盖不到的子系统。

## 什么时候你需要新的

**4 个条件全满足**才该加：

1. 这类任务至少每周一次。
2. 它有 multi-step workflow 而且对 agent 不明显。
3. 它的 voice 或格式规则和你默认的不同。
4. 现有 workstation 没覆盖。

如果只满足 1–2 个，把规则叠进现有 workstation 或根 `CLAUDE.md`。Workstation 是给完整 *子系统* 的，不是给每个偏好的。

## 步骤

### 1. 建文件夹

```
mkdir "Your Workstation Name"
mkdir "Your Workstation Name/Your Workstation Name Resources"
```

文件夹名是人类可读的，不是 slugified。文件夹名会出现在 agent 提及 workstation 的对话里。

### 2. 建 `CLAUDE.md`

模板：

```markdown
---
last-updated: YYYY-MM-DD
---

# <Workstation Name> - Workstation Rules

## Identity

一段话：这个 workstation 是谁，什么 routes 到这，什么不 route。

## Resources

| Resource | Read when... |
|---|---|
| `00_Resources/voice-principles.md` | 起草任何内容之前 |
| (随着你创建 workstation 特定 resource 加进来) | ... |

## Workflow

1. <第一步>
2. <第二步>
3. **STOP** 需要用户批准才能继续。
4. <...>

## Editorial Rules

Follow my voice principles in `00_Resources/voice-principles.md`. 叠加的规则：

- <规则 1>
- <规则 2>

## Failure modes

- <失败 1>
- <失败 2>
```

### 3. 建 `MEMORY.md`

```markdown
# <Workstation Name> Memory

## Contacts
_(空)_

## Key Decisions
_(空)_

## Recent Activity
_(空)_
```

### 4. 把 workstation 加到根 `CLAUDE.md`

在 **Routing Map** 段加一行：

```
| <Workstation Name> | 当我需要 <这个 workstation 处理的事> |
```

### 5. 加到 `00_Resources/index.md`

把 workstation 加进主题索引。

### 6. 测试

开 session。说一句应该 route 到这的话。确认 agent 加载了 workstation 的 `CLAUDE.md`。如果没有，你的 `Identity` 段对 routing 触发器不够具体 — 重写。

## 反模式

- **预先创建 workstation。** 不要「以备后用」。空 workstation 会腐烂。等真实需要出现再建。
- **每个 domain 一个 workstation。** Workstation 不是话题 — 是 workflow。「投资」不是 workstation；是 `LifeOS/` 子文件夹。「投资研究」*可能* 是 workstation，如果它有特定 workflow（比如 5 步 research-and-decision 流）。
- **巨型 workstation。** 如果 workstation 的 `CLAUDE.md` 超过 200 行，它做得太多了。拆成 2 个有清晰 routing 的 workstation。

## 什么时候退役一个 workstation

退役条件：
- 60+ 天没 route 到这
- Workflow 已经被另一个 workstation 吸收
- 你已经自动化了那块工作，workstation 不再需要

退役做法：把 workstation 文件夹移到 `_archive/workstations/<date>/<name>/`。更新 Routing Map 和 index.md。Agent 停止加载它。
