# 全局规则

## 表达

- 对话用简体中文。行文按 STE100 风格：短句、一句一事、祈使句、同一概念用词一致。
- 一切表述和产物（注释、release note、提交说明、issue、PR 评论）只用文字：不用 emoji，不写「参考 xx」，不出现 "seam"。
- 回复先给结果。简单问题用一到三句。不写开场，不复述问题，不逐步叙述工具过程，不在结尾再总结一遍。用户要求展开，或需要报错、失败输出、安全警告、破坏性操作确认时，写完整。已改就说改了。
- 输出按理解成本升级：默认用列表。用户点名要图，或关系用列表说不清时，才读 **show-me**。用户点名整页，或调用 `/skill:answer-me-with-html` 时，才出 HTML。不要默认打开浏览器。不要写入 `[answer-me-with-html always-on]`。show-me 只管内嵌在回答里的图（mermaid、伪代码、调用树、diff）。

## 代码质量

- 先读改动触及的代码并走通真实流程，再停在第一级够用的方案：能跳过、能复用（仓库已有 > 标准库 > 平台原生 >
  已装依赖）、能一行就不写多行；否则只写能跑的最少代码。
- 优先复用成熟库、组件源代码可控、统一设计语言、严禁在已有依赖可以正确表达的情况下自行设计实现组件和行为。
- 修缺陷改共享路径上的根因，不给每个调用方打补丁。不请求的抽象、预留脚手架和聪明写法不要；能删就别加，同等长度选边例正确的那个。删看似有意的功能前先问。

## 验证与执行

- 分析、核对、提问，或用户说先不改时：不改文件、不 commit、不启动服务。
- 分析、编排、验收在主会话做；执行后端确认可用且任务可独立验收时才派 subagent。
- subagent 的模型分工写在 agent 文件里：coder 用 `magpie/group/code-models`，explorer 用 `magpie/group/flash-models`。
- 判断 subagent 实际用的模型，只看回报头部的 `Model:` 行。agent 自述的模型标识不做证据，它可能报出上游成员模型的内部名或凭空编一个。
- 不主动跑 build 或完整测试套件，除非用户要求。
- 改代码后先格式化本会话改过的文件，再跑项目检查，修复本次引入的错误和警告；文档改动除外。
- 临时脚本写临时文件，跑完删除。

## 依赖

- 依赖和 lockfile 变更按待审查代码对待；直接依赖锁精确版本。
- 安装依赖时禁用生命周期脚本，除非用户允许。
- 升级关键依赖前读 changelog，评估后再应用。

## Git 与 PR

- 提交说明先看 `git log -10 --pretty=%s`。仓库已有风格就沿用。没有固定风格时用 Conventional Commits：`<type>(<scope>): <中文描述>`，scope 可省，type 只用 `feat`、`fix`、`refactor`、`perf`、`test`、`docs`、`build`、`ci`、`chore`、`style`、`revert`。文档用 `docs:`。发布提交沿用仓库已有的发布提交；没有时用 `chore(release): 发布 v<version>`，并沿用已有 tag 和标题。
- 提交前 `git log -10 --pretty=%s` 核类型格式；只 `git add` 本会话改过的路径，提交前 `git status` 核暂存。
- 不用 `git reset --hard`、`git checkout .`、`git clean -fd`、`git stash`、`--no-verify`，不 force push。
- 本地有未推送提交时，合远端优先 merge；`--rebase` 会把本地提交重放到远端之后，作者时间不变，时间线与拓扑顺序错开。
- rebase 冲突只解决自己改过的文件；他人文件的冲突中止并问用户。
- 合入、rebase、reset 前先看工作区：未提交路径与将写入路径有交集就停下问用户，禁止 `restore`/`checkout` 清掉再合；合完 `git status` 核同一批路径还在。备份只用 `git diff`/`git show` 写文件，apply 失败保留完整副本，禁止 `restore` 覆盖。
- 多行评论写临时文件，用 `--body-file` 发布。
- 关 issue 的 commit 带 `fixes #<n>` 或 `closes #<n>`；多个 issue 逐个写关键字（共享关键字只关闭第一个）。
