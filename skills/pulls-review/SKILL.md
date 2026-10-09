---
name: pulls-review
description: >
  用 pulls.review 把一次 diff 做成浏览器里分组、带摘要的审查。触发：用户说
  「pulls review」「用 pulls.review 看一下」「审查这个 diff / 分支 / 提交」，或给出
  `owner/repo#123`、github.com 的 PR、compare、commit 链接要求审查。
user-invocable: true
disable-model-invocation: true
---

用 `npx pulls.review` 在浏览器里审查一次 diff。它在 `localhost` 起一个页面，按改动分组并给出摘要。不安装，每次用 `npx`。

## 选 target

在仓库根目录跑。target 只决定打开哪一页，其他范围可以在页面里再选。按用户给的目标选一条，没给就用第一条：

| 用户给的 | 命令 |
| --- | --- |
| 没指定 | `npx pulls.review --port 7390` |
| 某个分支相对默认分支 | `npx pulls.review --port 7390 <branch>` |
| 分叉点之后新增的改动 | `npx pulls.review --port 7390 <base>...<head>` |
| 两个版本之间的树 diff | `npx pulls.review --port 7390 <A>..<B>` |
| 单个 commit | `npx pulls.review --port 7390 <rev>` |
| 未提交的改动，含未跟踪文件 | `npx pulls.review --port 7390 --worktree` |
| GitHub PR、compare、commit 或仓库链接 | `npx pulls.review --port 7390 owner/repo#123` 或直接给 URL |

## 步骤

1. 确认当前目录是 git 仓库。不是就停下来告诉用户。
2. 用户要看未提交改动时用 `--worktree`。工作区是脏的但用户没说要看它时，先问，不默认带上。
3. 跑上表对应的命令，一律带 `--port 7390`。页面设置存在浏览器 `localStorage`，按源隔离；端口每次变化会被当成新源，设置全部丢失。7390 是 CLI 的默认端口，固定它设置才保留。该端口被其他程序占用时换一个并告诉用户，不要退回随机端口。用户明确要求不打开浏览器时才加 `--no-open`。
4. 把终端打印的本地地址告诉用户。页面用终端里打印的一次性 code 做信任校验，不要自己编。

## 限制

- AI 分析不在命令行配置。页面 Settings 里选 API key，或选 Local agent，用本机已登录的 Claude Code、OpenCode 或 Pi。
- GitHub 目标的 token 取 `GITHUB_TOKEN`，没有就取 `gh auth token`。两者都没有时告诉用户，不替用户登录。
- 审查标记和 AI 结果缓存在仓库的 `.git/pulls-review`，不要提交它。
- 在 CI 里分析 GitHub PR 用的是另一个包 `@pulls.review/actions`，不在这个 skill 的范围里。
