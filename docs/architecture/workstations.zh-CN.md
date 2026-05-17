# Workstations

带自己规则、voice、workflow 的子系统。每个 workstation 是一个文件夹，里面有自己的 `CLAUDE.md`，它 **叠加在** 根宪法之上。

## 为什么需要 workstation

不同输出类型需要不同规则。起草长文（Writing HQ）的 workflow 和分流收件箱（Email HQ）、把技术诀窍变成指南（Tutorials）不一样。全塞进根 `CLAUDE.md` 会造出一个没人读的 1000 行宪法。

Workstation 让你根宪法保持短，只在你真的在那个 domain 工作时加载域特定规则。

## 模式

每个 workstation 有 3 样东西：

1. **`CLAUDE.md`** — workstation 规则，按这个顺序：
   - Identity（这 workstation 是谁，什么 routes 到这）
   - Resources（"Resource | Read when..." 表）
   - Workflow（编号步骤，STOP 门标出来）
   - Editorial Rules（开头总是：「Follow voice principles in `00_Resources/voice-principles.md`」）

2. **`MEMORY.md`** — workstation 专属记忆：
   - Contacts（这个 domain 里的人）
   - Key Decisions（workstation 特定选择的理由）
   - Recent Activity（运行日志）

3. **`<Workstation> Resources/`** 文件夹 — 模板、brief、人物笔记、所有域特定东西

## 什么时候加 workstation

加的条件：
- 某类任务反复出现（≥ 每周）
- 它有多步骤 workflow 而且对 agent 不明显
- 它有和默认不同的 voice / 格式规则

**不要**加，如果：
- 任务是一次性的（没有重复结构）
- 「规则」只是已经被根 `CLAUDE.md` 覆盖的偏好

## Template 自带的 workstation

- **Writing HQ**（`Writing/Writing HQ/`）— 9 步长文起草带 STOP 门。
- **Email HQ**（`Email HQ/`）— 收件箱分流 + thread-aware 回信起草。
- **Tutorials**（`Tutorials/`）— 把技术诀窍变成非工程师友好的指南。

## 什么时候合并 workstation

合并条件：
- 两个 workstation 规则严重重叠
- 一个 workstation 一年只用 1–2 次（把内容并到最相似的活跃 workstation）
- voice 规则正在收敛

## 怎么加你自己的

完整流程见 [docs/customization/adding-a-workstation.zh-CN.md](../customization/adding-a-workstation.zh-CN.md)。
