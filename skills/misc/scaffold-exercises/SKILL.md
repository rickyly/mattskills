---
name: scaffold-exercises
description: 创建包含章节、习题、解答与讲解且能通过 lint 检查的练习目录结构。当用户希望搭建练习脚手架、创建练习骨架，或建立新的课程章节时使用。
---

# 搭建练习脚手架

创建能通过 `pnpm ai-hero-cli internal lint` 检查的练习目录结构，然后用 `git commit` 提交。

## 目录命名

- **章节**：`exercises/` 内的 `XX-section-name/`（例如 `01-retrieval-skill-building`）
- **练习**：章节内的 `XX.YY-exercise-name/`（例如 `01.03-retrieval-with-bm25`）
- 章节编号 = `XX`，练习编号 = `XX.YY`
- 名称采用 dash-case（小写、连字符）

## 练习变体

每个练习至少需要以下子文件夹中的一个：

- `problem/` —— 含 TODO 的学员工作区
- `solution/` —— 参考实现
- `explainer/` —— 概念性材料，无 TODO

搭建骨架时，除非计划另有指定，否则默认使用 `explainer/`。

## 必需文件

每个子文件夹（`problem/`、`solution/`、`explainer/`）都需要一个 `readme.md`，要求：

- **非空**（必须有实际内容，哪怕只有一行标题也行）
- 没有失效的链接

搭建骨架时，创建一个仅含标题和描述的最小 readme：

```md
# Exercise Title

Description here
```

如果子文件夹含有代码，它还需要一个 `main.ts`（多于 1 行）。但对于骨架，只含 readme 的练习即可。

## 工作流

1. **解析计划** —— 提取章节名、练习名与变体类型
2. **创建目录** —— 对每个路径执行 `mkdir -p`
3. **创建 readme 骨架** —— 每个变体文件夹一个带标题的 `readme.md`
4. **运行 lint** —— 用 `pnpm ai-hero-cli internal lint` 校验
5. **修复所有错误** —— 反复迭代直到 lint 通过

## Lint 规则摘要

该 linter（`pnpm ai-hero-cli internal lint`）会检查：

- 每个练习都有子文件夹（`problem/`、`solution/`、`explainer/`）
- `problem/`、`explainer/` 或 `explainer.1/` 至少存在一个
- 主子文件夹中存在 `readme.md` 且非空
- 没有 `.gitkeep` 文件
- 没有 `speaker-notes.md` 文件
- readme 中没有失效的链接
- readme 中没有 `pnpm run exercise` 命令
- 除非只含 readme，否则每个子文件夹都需要 `main.ts`

## 移动 / 重命名练习

重新编号或移动练习时：

1. 使用 `git mv`（而非 `mv`）重命名目录——可保留 git 历史
2. 更新数字前缀以维持顺序
3. 移动后重新运行 lint

示例：

```bash
git mv exercises/01-retrieval/01.03-embeddings exercises/01-retrieval/01.04-embeddings
```

## 示例：从计划生成骨架

给定如下计划：

```
Section 05: Memory Skill Building
- 05.01 Introduction to Memory
- 05.02 Short-term Memory (explainer + problem + solution)
- 05.03 Long-term Memory
```

创建：

```bash
mkdir -p exercises/05-memory-skill-building/05.01-introduction-to-memory/explainer
mkdir -p exercises/05-memory-skill-building/05.02-short-term-memory/{explainer,problem,solution}
mkdir -p exercises/05-memory-skill-building/05.03-long-term-memory/explainer
```

然后创建 readme 骨架：

```
exercises/05-memory-skill-building/05.01-introduction-to-memory/explainer/readme.md -> "# Introduction to Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/explainer/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/problem/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/solution/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.03-long-term-memory/explainer/readme.md -> "# Long-term Memory"
```
