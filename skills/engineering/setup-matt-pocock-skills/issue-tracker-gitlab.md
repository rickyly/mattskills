# Issue 跟踪器：GitLab

本仓库的 issue 和 PRD 以 GitLab issue 的形式存放。所有操作都使用 [`glab`](https://gitlab.com/gitlab-org/cli) CLI。

## 约定

- **创建 issue**：`glab issue create --title "..." --description "..."`。多行描述使用 heredoc。传入 `--description -` 可打开编辑器。
- **读取 issue**：`glab issue view <number> --comments`。用 `-F json` 获取机器可读的输出。
- **列出 issue**：`glab issue list -F json`，配合适当的 `--label` 过滤器。
- **在 issue 上评论**：`glab issue note <number> --message "..."`。GitLab 把评论称为「note」。
- **应用 / 移除标签**：`glab issue update <number> --label "..."` / `--unlabel "..."`。多个标签可以用逗号分隔，或通过重复该 flag 指定。
- **关闭**：`glab issue close <number>`。`glab issue close` 不接受关闭评论，所以先用 `glab issue note <number> --message "..."` 发布说明，再关闭。
- **Merge request**：GitLab 把 PR 称为「merge request」。使用 `glab mr create`、`glab mr view`、`glab mr note` 等——形态与 `gh pr ...` 相同，只是用 `mr` 代替 `pr`，用 `note`/`--message` 代替 `comment`/`--body`。

从 `git remote -v` 推断仓库——`glab` 在克隆体内运行时会自动完成这一点。

## Merge request 作为 triage 面

**MR 作为请求来源：否。** _(如果本仓库把外部 merge request 视为功能请求，设为 `yes`；`/triage` 会读取此标志。)_

设为 `yes` 时，MR 会走与 issue 相同的标签和状态，使用 `glab mr` 等价命令：

- **读取 MR**：`glab mr view <number> --comments`，以及用 `glab mr diff <number>` 查看 diff。
- **列出待 triage 的外部 MR**：`glab mr list -F json`，然后只保留作者不是项目成员 / 所有者的 MR（贡献者的 MR，而非维护者进行中的工作）。
- **评论 / 打标签 / 关闭**：`glab mr note`、`glab mr update --label`/`--unlabel`、`glab mr close`。

与 GitHub 不同，GitLab 对 issue 和 MR 分别编号，所以一旦你知道维护者指的是哪个面，`#42` 就没有歧义。

## 当某个技能说「发布到 issue 跟踪器」

创建一个 GitLab issue。

## 当某个技能说「获取相关工单」

运行 `glab issue view <number> --comments`。
