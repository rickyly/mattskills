# Matt Pocock Skills

A collection of agent skills (slash commands and behaviors) loaded by Claude Code. Skills are organized into buckets and consumed by per-repo configuration emitted by `/setup-matt-pocock-skills`.

## 语言

**Issue tracker**：
托管某个仓库 issue 的工具——GitHub Issues、Linear、本地 `.scratch/` markdown 约定或类似工具。`to-issues`、`to-prd`、`triage` 和 `qa` 等技能从中读取并向其写入。
_避免_：backlog manager、backlog backend、issue host

**Issue**：
**Issue tracker** 内单个被跟踪的工作单元——一个 bug、任务、PRD，或由 `to-issues` 产出的切片。
_避免_：ticket（仅在引用把它们称为 ticket 的外部系统时使用）

**Triage role**：
triage 期间施加到 **Issue** 上的规范状态机标签（如 `needs-triage`、`ready-for-afk`）。每个角色通过 `docs/agents/triage-labels.md` 映射到 **Issue tracker** 中真实的标签字符串。

## 关系

- 一个 **Issue tracker** 持有多个 **Issue**
- 一个 **Issue** 在同一时刻携带一个 **Triage role**

## 已标记的歧义

- “backlog” 此前既用于指代托管 issue 的*工具*，又用于指代其内部的*工作主体*——已解决：工具是 **Issue tracker**；“backlog” 不再作为领域术语使用。
- “backlog backend” / “backlog manager”——已解决：合并为 **Issue tracker**。
