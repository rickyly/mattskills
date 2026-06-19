---
name: setup-matt-pocock-skills
description: 为本仓库配置工程类技能——设置其 issue 跟踪器、triage 标签词汇表和领域文档布局。首次使用其他工程类技能前运行一次。
disable-model-invocation: true
---

# 配置 Matt Pocock 的技能

搭建工程类技能所依赖的、按仓库划分的配置：

- **Issue 跟踪器**——issue 存放在哪里（默认 GitHub；开箱即用也支持本地 markdown）
- **Triage 标签**——五种规范 triage 角色所用的字符串
- **领域文档**——`CONTEXT.md` 和 ADR 存放在哪里，以及读取它们的消费方规则

这是一个由提示驱动的技能，而非确定性脚本。先探查、再展示你的发现、与用户确认，最后再写入。

## 流程

### 1. 探查

查看当前仓库以了解其初始状态。读取已有的内容，不要臆断：

- `git remote -v` 和 `.git/config`——这是一个 GitHub 仓库吗？是哪一个？
- 仓库根目录的 `AGENTS.md` 和 `CLAUDE.md`——其中是否存在某一个？其中是否已有 `## Agent skills` 小节？
- 仓库根目录的 `CONTEXT.md` 和 `CONTEXT-MAP.md`
- `docs/adr/` 以及任何 `src/*/docs/adr/` 目录
- `docs/agents/`——本技能之前的输出是否已经存在？
- `.scratch/`——表明已经在使用本地 markdown 的 issue 跟踪器约定

### 2. 展示发现并询问

总结哪些已存在、哪些缺失。然后**逐个**带用户走过这三项决策——展示一个小节，得到用户的回答，再进入下一个。不要把三项一次性全抛出来。

假设用户并不知道这些术语的含义。每个小节都以一段简短的说明开头（它是什么、为什么这些技能需要它、选择不同会带来什么变化）。然后展示各个选项和默认值。

**小节 A——Issue 跟踪器。**

> 说明：「issue 跟踪器」是本仓库 issue 的存放之处。诸如 `to-issues`、`triage`、`to-prd` 和 `qa` 等技能会从中读取、向其中写入——它们需要知道是调用 `gh issue create`、在 `.scratch/` 下写一个 markdown 文件，还是遵循你描述的其他工作流。选择你实际为本仓库跟踪工作的地方。

默认姿态：这些技能是为 GitHub 设计的。如果某个 `git remote` 指向 GitHub，就建议它。如果某个 `git remote` 指向 GitLab（`gitlab.com` 或自托管主机），就建议 GitLab。否则（或者用户更偏好其他方式），提供以下选项：

- **GitHub**——issue 存放在仓库的 GitHub Issues 中（使用 `gh` CLI）
- **GitLab**——issue 存放在仓库的 GitLab Issues 中（使用 [`glab`](https://gitlab.com/gitlab-org/cli) CLI）
- **本地 markdown**——issue 以本仓库 `.scratch/<feature>/` 下的文件形式存放（适合个人项目或没有 remote 的仓库）
- **其他**（Jira、Linear 等）——请用户用一段话描述工作流；本技能会将其记录为自由格式的散文

当且仅当用户选择了 **GitHub** 或 **GitLab** 时，再问一个追问：

> 说明：开源仓库经常以 pull request 而非仅仅 issue 的形式收到功能请求——一个 PR 就是附带代码的 issue。如果你开启此项，`/triage` 会把*外部* PR 拉入同一个队列，并让它们走与 issue 相同的标签和状态（协作者进行中的 PR 不受影响）。如果 PR 对你而言不是请求来源，就保持关闭。

- **PR 作为请求来源**——是 / 否（默认：否）。把答案记录在 `docs/agents/issue-tracker.md` 中。对于本地 markdown 和其他跟踪器，跳过这个问题——它们没有 PR。

**小节 B——Triage 标签词汇表。**

> 说明：当 `triage` 技能处理一个新进来的 issue 时，它会让该 issue 走一个状态机——需要评估、等待报告者、可供 AFK 代理领取、可供人类领取，或不予修复。要做到这一点，它需要应用与*你实际配置过的*字符串相匹配的标签（或你的 issue 跟踪器中的等价物）。如果你的仓库已经使用了不同的标签名（例如 `bug:triage` 而非 `needs-triage`），就在这里映射它们，这样技能会应用正确的标签，而不是创建重复项。

五种规范角色：

- `needs-triage`——维护者需要评估
- `needs-info`——等待报告者
- `ready-for-agent`——已完整规约、可供 AFK 领取（代理无需任何人类上下文即可领取）
- `ready-for-human`——需要人类实现
- `wontfix`——不会被处理

默认：每个角色的字符串等于其名称。询问用户是否想覆盖其中任何一个。如果他们的 issue 跟踪器没有已有标签，默认值就够用了。

**小节 C——领域文档。**

> 说明：某些技能（`improve-codebase-architecture`、`diagnosing-bugs`、`tdd`）会读取 `CONTEXT.md` 文件以学习项目的领域语言，并读取 `docs/adr/` 以了解过往的架构决策。它们需要知道仓库是单一上下文还是多上下文（例如一个前端 / 后端上下文分离的 monorepo），以便在正确的位置查找。

确认布局：

- **单上下文**——仓库根目录有一个 `CONTEXT.md` 加 `docs/adr/`。大多数仓库都属于这种。
- **多上下文**——根目录有 `CONTEXT-MAP.md`，指向按上下文划分的各个 `CONTEXT.md` 文件（通常是 monorepo）。

### 3. 确认并编辑

向用户展示一份草稿：

- 要添加到 `CLAUDE.md` / `AGENTS.md` 中被编辑文件的 `## Agent skills` 区块（选择规则见步骤 4）
- `docs/agents/issue-tracker.md`、`docs/agents/triage-labels.md`、`docs/agents/domain.md` 的内容

让他们在写入前进行编辑。

### 4. 写入

**选择要编辑的文件：**

- 如果 `CLAUDE.md` 存在，编辑它。
- 否则如果 `AGENTS.md` 存在，编辑它。
- 如果两者都不存在，询问用户要创建哪一个——不要替他们做决定。

当 `CLAUDE.md` 已存在时，绝不创建 `AGENTS.md`（反之亦然）——始终编辑已经存在的那一个。

如果所选文件中已存在 `## Agent skills` 区块，就就地更新其内容，而不是追加一个重复项。不要覆盖用户对周边小节的编辑。

该区块：

```markdown
## Agent skills

### Issue tracker

[one-line summary of where issues are tracked, plus whether external PRs are a triage surface]. See `docs/agents/issue-tracker.md`.

### Triage labels

[one-line summary of the label vocabulary]. See `docs/agents/triage-labels.md`.

### Domain docs

[one-line summary of layout — "single-context" or "multi-context"]. See `docs/agents/domain.md`.
```

然后以本技能文件夹中的种子模板为起点，写入这三个文档文件：

- [issue-tracker-github.md](./issue-tracker-github.md)——GitHub issue 跟踪器
- [issue-tracker-gitlab.md](./issue-tracker-gitlab.md)——GitLab issue 跟踪器
- [issue-tracker-local.md](./issue-tracker-local.md)——本地 markdown issue 跟踪器
- [triage-labels.md](./triage-labels.md)——标签映射
- [domain.md](./domain.md)——领域文档消费方规则加布局

对于「其他」issue 跟踪器，用用户的描述从头编写 `docs/agents/issue-tracker.md`。

### 5. 完成

告诉用户设置已完成，以及现在哪些工程类技能会从这些文件中读取。提醒他们以后可以直接编辑 `docs/agents/*.md`——只有当他们想切换 issue 跟踪器或从头重新开始时，才需要重新运行本技能。
