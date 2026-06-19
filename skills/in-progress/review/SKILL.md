---
name: review
description: 沿两条轴审查自某一固定点（commit、分支、tag 或 merge-base）以来的变更——Standards（代码是否遵循本仓库已记录的编码标准？）与 Spec（代码是否符合原始 issue/PRD 的要求？）。在并行的子代理中分别运行两项审查，并将结果并排报告。当用户想审查一个分支、一个 PR、进行中的变更，或要求「review since X」时使用。
---

对 `HEAD` 与用户提供的固定点之间的 diff 进行双轴审查：

- **Standards** —— 代码是否符合本仓库已记录的编码标准？
- **Spec** —— 代码是否忠实实现了原始的 issue / PRD / 规格？

两条轴都作为**并行子代理**运行，使它们不会污染彼此的上下文，然后本技能汇总它们的发现。

issue 跟踪器应当已经提供给你——如果 `docs/agents/issue-tracker.md` 缺失，运行 `/setup-matt-pocock-skills`。

## 流程

### 1. 钉住固定点

用户所说的就是固定点——一个 commit SHA、分支名、tag、`main`、`HEAD~5` 等等。如果他们没有指定，就询问。

把 diff 命令记录一次：`git diff <fixed-point>...HEAD`（三个点，因此比较是针对 merge-base 的）。同时通过 `git log <fixed-point>..HEAD --oneline` 记下提交列表。

在继续之前，确认固定点能解析（`git rev-parse <fixed-point>`）且 diff 非空。坏的 ref 或空 diff 应当在这里就失败——而不是在两个并行子代理内部。

### 2. 识别规格来源

按以下顺序查找原始规格：

1. 提交信息中的 issue 引用（`#123`、`Closes #45`、GitLab `!67` 等）——通过 `docs/agents/issue-tracker.md` 中的工作流获取。
2. 用户作为参数传入的路径。
3. `docs/`、`specs/` 或 `.scratch/` 下与分支名或特性匹配的 PRD/规格文件。
4. 如果什么都没找到，询问用户规格在哪里。如果他们说没有，**Spec** 子代理就跳过并报告「no spec available」。

### 3. 识别标准来源

仓库中任何记录代码应当如何编写的文件，例如 `CODING_STANDARDS.md` 或 `CONTRIBUTING.md`。

### 4. 并行派生两个子代理

发送一条带两个 `Agent` 工具调用的消息。两者都使用 `general-purpose` 子代理。

**Standards 子代理提示词** —— 包含：

- 完整的 diff 命令和提交列表。
- 你在第 3 步找到的标准来源文件列表。
- 要点说明："Report — per file/hunk where relevant — every place the diff violates a documented standard. Cite the standard (file + the rule). Distinguish hard violations from judgement calls. Skip anything tooling enforces. Under 400 words."

**Spec 子代理提示词** —— 包含：

- diff 命令和提交列表。
- 规格的路径或已获取的内容。
- 要点说明："Report: (a) requirements the spec asked for that are missing or partial; (b) behaviour in the diff that wasn't asked for (scope creep); (c) requirements that look implemented but where the implementation looks wrong. Quote the spec line for each finding. Under 400 words."

如果规格缺失，就跳过 Spec 子代理并在最终报告中注明。

### 5. 汇总

在 `## Standards` 和 `## Spec` 标题下呈现两份报告，原样或略加整理。**不要**合并或重新排列发现的问题——这两条轴是刻意分开的（见 _Why two axes_）。

以一行总结收尾：每条轴的发现总数，以及_每条轴内部_最严重的问题（若有）。不要跨轴挑出唯一的「赢家」——那正是这种分离要防止的重新排序。

## 为什么用两条轴

一处变更可能通过一条轴而在另一条轴上失败：

- 遵循了每一条标准却实现了错误东西的代码 → **Standards 通过，Spec 失败。**
- 完全照 issue 要求去做但破坏了项目约定的代码 → **Spec 通过，Standards 失败。**

分开报告能阻止一条轴遮蔽另一条轴。
