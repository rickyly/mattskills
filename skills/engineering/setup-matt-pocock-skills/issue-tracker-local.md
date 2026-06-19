# Issue 跟踪器：本地 Markdown

本仓库的 issue 和 PRD 以 `.scratch/` 中的 markdown 文件形式存放。

## 约定

- 每个功能一个目录：`.scratch/<feature-slug>/`
- PRD 是 `.scratch/<feature-slug>/PRD.md`
- 实现 issue 是 `.scratch/<feature-slug>/issues/<NN>-<slug>.md`，从 `01` 开始编号
- Triage 状态记录为每个 issue 文件顶部附近的一行 `Status:`（角色字符串见 `triage-labels.md`）
- 评论和对话历史追加到文件底部 `## Comments` 标题下

## 当某个技能说「发布到 issue 跟踪器」

在 `.scratch/<feature-slug>/` 下创建一个新文件（如有需要则创建该目录）。

## 当某个技能说「获取相关工单」

读取所引用路径处的文件。用户通常会直接传入路径或 issue 编号。
