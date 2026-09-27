---
name: lody-review
description: >
  lody review: turn a PR, commit range, or checkout into a local HTML review
  via `npx lody review`. Use when the user asks to review with lody, run
  `npx lody review`, or wants a shareable `.review.html` of git changes.
---

把一次 Git 审查做成可打开的 HTML。格式以当场跑出的 **prompt** 为准，不靠记忆里的模板。

## Steps

1. **Load the prompt.** 在仓库里跑 `npx lody review prompt`。那份输出是 `.review.md` 的唯一规格：frontmatter、分组、`changes://`、`P0`/`P1`/`P2`、行号规则都按它写。完成：prompt 已在本次上下文里，后续步骤按它执行。

2. **Lock the range.** 用户给的目标优先：PR、分支、单个 commit、`base..head`、近 N 个 commit。`git status --short` 非空且用户还没选定脏工作区怎么处理时，先问再写文件。完成：`merge_base` 与 `current_commit` 都是完整 SHA；未提交文件要么按用户要求进了 commit，要么排除在审查之外。空差分（两 SHA 相同）停下来告诉用户，不写空稿。

3. **Write the review.** 按 prompt 收集 commit、diff、周边代码。审查语言跟用户说话的语言。行号和文件内容从 `current_commit` 取（`git show <sha>:path`），不读工作区脏副本。写一份 `*.review.md` 到系统临时目录（Windows `%TEMP%`，Unix `$TMPDIR`）。完成：frontmatter 的两个 SHA 与第 2 步一致；每个逻辑组用 `changes://` 点到它覆盖的已改文件；每个 `P0`/`P1`/`P2` 带可跳转的 `new://` 或 `old://` 引用。

4. **Render.** 在仓库内执行 `npx lody review <file> --repo <repo>`。若 CLI 拉不到与自身版本一致的 `lody-code-review-viewer`（404 或 sha256 mismatch），用 `npm view lody-code-review-viewer version` 取已发布版本，再跑 `npx lody@<该版本> review <file> --repo <repo>`。prompt 点名的 validate 命令不存在就直接渲染。完成：CLI 打出 HTML 路径，并把 `.review.md` 与 `.review.html` 的绝对路径都告诉用户。两份都留在临时目录。
