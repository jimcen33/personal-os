# 为什么每个 skill 一个文件夹

每个 skill 住自己文件夹：`.claude/skills/<name>/SKILL.md`。不是 `.claude/skills/<name>.md`（单文件），也不是所有 skill 塞一个文件。

这是刻意的。

## 理由

### 1. Skill 需要附件
有些 skill 需要模板文件、参考数据、样本输入。文件夹让你把 `SKILL.md` + `template.md` + `sample.json` + 给贡献者的 `README.md` 同处。

### 2. Skill 版本化
`last-updated` 和 `version` frontmatter 字段在每个 skill 是自包含单元时工作更好。14 个 skill 共享一个文件，bump 某一个的版本会很乱。

### 3. Skill 归属
多贡献者世界里，「拥有 ingest skill」映射到「拥有 `.claude/skills/ingest/`」。比「拥有 skills.md 42–218 行」干净。

### 4. 未来 plugin 打包
你以后想把一个 skill 抽成 plugin 或上 marketplace，住在文件夹里让抽出来变成一行 `mv`。

## skill 文件夹里放什么

必须：
- `SKILL.md` - skill 定义

可选：
- `template.md` - skill 用的参考模板
- `examples/` - 样本输入和输出
- `README.md` - 给贡献者的笔记（agent 不加载）
- `tests/` - 如果你写 skill 评估套件

## 什么 **不** 放进 skill 文件夹

- 用户数据
- 生成的输出
- 日志
- 任何按 session 变化的东西

那些放在 wiki 的对应层，不在 skill 文件夹。
