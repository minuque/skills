# skills

个人技能集合，每个目录一个技能，遵循 `SKILL.md`（YAML frontmatter + 正文）约定，可用于 Claude Code、pi、Codex 等支持该约定的 agent。

## 技能

- **agent-browser** — 浏览器或 Electron 交互、截图、表单操作、浏览器验收。仅在用户明确要求时触发。
- **answer-me-with-html** — 把一次回答变成一页带结论的可视化 HTML 讲解。
- **bro** — 用大白话复述上一条消息，去掉术语。
- **grill-me** — 就一个方案、决定或想法连环追问，把思路逼清楚。
- **handoff** — 把当前会话压缩成交接文档，供下一个 agent 接手。
- **herdr** — 控制 Herdr 终端复用器，检查或操作 pane、tab、workspace 和其他 agent。仅在用户明确提到 Herdr 时使用。
- **land-with-subagents** — 把已定方案拆成可独立验收的任务派给 subagent，编排者只做验证和验收。
- **no-commit-yet** — 本轮会话暂不 git commit。
- **no-patch-optimal** — 在共享路径上按最优解法改根因，不打补丁。
- **ponytail-review** — 过度设计审查：只查臃肿与 YAGNI，不改代码。
- **pulls-review** — 用 `npx pulls.review` 把一次 diff 做成浏览器里分组、带摘要的审查。
- **show-me** — 用示意图、代码形状草图和聚焦的 HTML 产物把当前话题讲清楚。

## 使用

整体克隆到 agent 的技能目录：

```bash
git clone https://github.com/minuque/skills.git ~/.agents/skills
```

也可以只取用单个技能，把对应目录复制进自己的技能目录即可。
