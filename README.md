# skills

agent 与技能的配置仓库：`AGENTS.md` 是全局规则，`skills/` 下的每个目录是一个技能，遵循 `SKILL.md`（YAML frontmatter + 正文）约定，可用于 Claude Code、pi、Codex 等支持该约定的 agent。

## 技能

- **bro** — 用大白话复述上一条消息，去掉术语。
- **grill-me** — 就一个方案、决定或想法连环追问，把思路逼清楚。
- **handoff** — 把当前会话压缩成交接文档，供下一个 agent 接手。
- **no-commit-yet** — 本轮会话暂不 git commit。
- **ponytail-review** — 过度设计审查：只查臃肿与 YAGNI，不改代码。
- **show-me** — 用内嵌示意图和代码形状草图把当前话题讲清楚。

## 使用

整体克隆到 agent 的根目录：

```bash
git clone https://github.com/minuque/skills.git ~/.agents
```

也可以只取用单个技能，把 `skills/` 下对应目录复制进自己的技能目录即可。
