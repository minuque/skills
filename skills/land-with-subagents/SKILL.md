---
name: land-with-subagents
description: >
  把当前已定方案拆成可独立验收的任务，派发 subagent 落地，编排者只做验证和验收。
  触发：「派发几个subagent实施，你负责验证和验收」「ok 拉个subagent实施」「分配几个subagent，按prd落地」。
user-invocable: true
---

用户要派 subagent 落地、你负责验收时用。用 `Agent`。类型只用 `coder`、`explorer`、`reviewer`。不要用 `general-purpose`。隔离省略；用户要隔离才写 `isolation: worktree`。

1. 只派已经谈定、可独立验收的项。边界不清先问，不要把「先分析」项派下去改文件。
2. 每个 subagent 一个逻辑任务。可并行的同时派。编排者自己不写那些文件。
3. 收回来后你验证：读 diff、跑项目对该任务要求的检查。浏览器验收用已连接的 `chrome-devtools`；没有就跳过，不要找 Playwright。
4. 按 prd 或讨论清单派。
5. 收尾说明每个任务做了什么、哪项没做。
