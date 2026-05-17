# Multi-Vault：长成 constellation

Template 出厂是单 vault。真实的 founder workflow 经常会长成一组按敏感度和受众分的 vault。这篇讲那个模式。

## 什么时候你长出了单 vault

症状：
- 你有只属于你的材料（财务、招聘决策、合同）和团队协作材料（工程决策、产品 spec）混在一起。不同受众，同一 wiki。
- 你想从 wiki 里发部分内容到公开，但文件系统让你很难判断哪些安全分享。
- 你同时用 wiki 装公司的和个人的，边界模糊。

两个以上摩擦出现，就到时候了。

## Constellation 模式

3 个 vault，每个有不同敏感度档案：

| Vault | 用途 | 受众 |
|---|---|---|
| `personal-os/`（这个 template） | 你个人的知识、voice、方法论 | 你 + Cowork |
| `company-wiki/` | 团队共享的产品、工程、ops 决策 | 你 + 同事 + Cowork |
| `private-wiki/` | 高敏：财务、招聘、合同、投资人关系 | 你 +（极少数）+ private-only session 里的 Cowork |

每个是独立 git repo。每个有自己的 `CLAUDE.md`、`MEMORY.md`、`AGENTS.md`。

## 跨 vault routing

Personal OS 的 `/ingest` skill，在 cross-vault 模式下，可以根据敏感度扫描自动 route 材料到正确 vault。

模式：

1. `Notes/Inbox/` 接收来自任意来源的材料
2. `/ingest` 读每项跑敏感度扫描
3. 标记为 company-相关的 → route 到 `company-wiki/Notes/Inbox/`，由那个 vault 的 ingest 处理
4. 标记为高敏的 → route 到 `private-wiki/Notes/Inbox/`
5. 个人材料留在 `personal-os/`，正常 ingest

敏感度扫描逻辑在 `00_Resources/sensitivity-rules.md`（这个 template 不出厂这个文件 — 你按自己的 context 写）。

## 沙箱约束

Cowork 在一个 vault 跑 session 但 mount 包含其他 vault 时，有实践约束：

- **写入 mount vault 的 `.git/objects/`** 可能失败（沙箱限制）。
- **Index 锁竞争** 会留下 stuck 的 `.git/index.lock`。
- **`git checkout -b`** 在 mount vault 上会 hang。

实用规则：**在 Cowork 里做文件编辑；所有 git 操作在本地 terminal 里做。** 如果 Cowork 试图在 mount vault 上 `git commit`，你得手动清 `.git/index.lock`。

## 单向 inbound 读取

一个 vault 的 session 可以 **读** 另一个 vault（跨 vault context）。不能 **写**。

例子：`company-wiki/` 里的 session 需要你的 voice principles。它直接读 `personal-os/00_Resources/voice-principles.md`。不要把文件复制到 company-wiki — 保持引用单点。

## Outbound 写入（唯一记录的例外）

Personal OS 的 `/ingest` skill 在 cross-vault routing 模式是唯一记录的跨 vault 写例外。它把 `personal-os/Notes/Inbox/` 里自动分类的材料 route 到 `company-wiki/Notes/Inbox/` 或 `private-wiki/Notes/Inbox/`。

任何其他跨 vault 写必须来自在目标 vault 文件夹里开的 session。

## Bundle delivery 模式（用于基础设施变更）

要把基础设施变更（新 skill、新模板、新 resource 文件、根 CLAUDE.md 编辑）从一个 vault 运到另一个时，模式是 **bundle 投递，不直接编辑**：

1. 源 vault 的 Cowork session 把新/改的文件写到 `Cowork Outputs/<change-slug>-<date>/`，镜像目标结构。
2. Bundle 包含 `README.md` 注明 apply 顺序和验证命令。
3. 你在 terminal 里手动 apply。

这避开了 mount 上 git 操作的沙箱问题，给你 apply 前 review 的机会。

## 怎么搭

1. 决定需要哪几个 vault（Personal + Company？Personal + Company + Private？）。
2. 每个新 vault：fork 或 clone `personal-os` 改名 — 是同一个 template，只是 scope 和内容不同。
3. 编辑每个 vault 的 `CLAUDE.md`：
   - Wiki Scope 声明窄到那个 vault 的 domain
   - 加 `Companion Vaults` 段列出其他 vault + 读写规则
4. 写 `00_Resources/sensitivity-rules.md` 装 routing 逻辑。
5. 更新 `personal-os/.claude/skills/ingest/SKILL.md` 加 cross-vault routing 步骤。

## 什么留在 Personal OS

即便在 constellation 里：
- 你的 voice principles
- 个人 Knowledge 层（不分组织的持久理解）
- LifeOS 层（钱、健康、联系人）
- 个人写作的 `Writing/Drafts/` 层
- Hot cache、spotlight log、entity dictionary

Personal OS 仍是你的 *主* vault。其他扩展它，不替换它。

## 什么时候 **不** 加更多 vault

- 单干、不分享给同事 — 一个 vault。
- 你的「团队」内容是一份每月更新的 Notion doc — 那是 Notion doc，不是 wiki。
- 你说不清一块内容属于 vault A 还是 vault B 的一句话理由 — 边界还不够清。保持单 vault 直到说得清。

过早多 vault 比晚多 vault 更痛。
