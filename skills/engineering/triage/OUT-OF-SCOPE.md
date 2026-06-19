# Out-of-Scope 知识库

仓库中的 `.out-of-scope/` 目录持久化保存被拒绝功能请求的记录。它服务于两个目的：

1. **Institutional memory** — 一个功能为什么被拒绝，这样在 issue 关闭后理由不会丢失
2. **Deduplication** — 当一个新 issue 进来且匹配某个先前的拒绝时，技能可以浮现先前的决定，而不必重新争论一遍

## 目录结构

```
.out-of-scope/
├── dark-mode.md
├── plugin-system.md
└── graphql-api.md
```

每个**概念（concept）**一个文件，而非每个 issue 一个。请求同一件事的多个 issue 归到同一个文件之下。

## 文件格式

文件应以一种轻松、易读的风格撰写——更像一篇简短的设计文档，而不是一条数据库记录。用段落、代码示例和例子把理由讲清楚，让第一次接触它的人也觉得有用。

```markdown
# Dark Mode

This project does not support dark mode or user-facing theming.

## Why this is out of scope

The rendering pipeline assumes a single color palette defined in
`ThemeConfig`. Supporting multiple themes would require:

- A theme context provider wrapping the entire component tree
- Per-component theme-aware style resolution
- A persistence layer for user theme preferences

This is a significant architectural change that doesn't align with the
project's focus on content authoring. Theming is a concern for downstream
consumers who embed or redistribute the output.

```ts
// The current ThemeConfig interface is not designed for runtime switching:
interface ThemeConfig {
  colors: ColorPalette; // single palette, resolved at build time
  fonts: FontStack;
}
```

## Prior requests

- #42 — "Add dark mode support"
- #87 — "Night theme for accessibility"
- #134 — "Dark theme option"
```

### 文件命名

为概念取一个简短、描述性的 kebab-case 名称：`dark-mode.md`、`plugin-system.md`、`graphql-api.md`。名称应足够易辨识，让浏览目录的人不用打开文件也能明白什么被拒绝了。

### 撰写理由

理由应当言之有物——不是"我们不想要这个"，而是为什么。好的理由会引用：

- 项目范围或理念（"This project focuses on X; theming is a downstream concern"）
- 技术约束（"Supporting this would require Y, which conflicts with our Z architecture"）
- 战略决策（"We chose to use A instead of B because..."）

理由应当经久耐用。避免引用临时性的处境（"we're too busy right now"）——那些不是真正的拒绝，而是推迟。

## 何时检查 `.out-of-scope/`

在分诊期间（第 1 步：Gather context），读 `.out-of-scope/` 中的所有文件。在评估一个新 issue 时：

- 检查该请求是否匹配某个既有的 out-of-scope 概念
- 匹配按概念相似度进行，而非关键词——"night theme" 匹配 `dark-mode.md`
- 如果有匹配，把它浮现给维护者："This is similar to `.out-of-scope/dark-mode.md` — we rejected this before because [reason]. Do you still feel the same way?"

维护者可以：

- **Confirm** — 新 issue 被加入既有文件的 "Prior requests" 列表，然后关闭
- **Reconsider** — 删除或更新这个 out-of-scope 文件，并让该 issue 走正常分诊流程
- **Disagree** — 这些 issue 相关但有所不同，继续走正常分诊

## 何时写入 `.out-of-scope/`

只有当一个**enhancement**（而非 bug）被*拒绝*为 `wontfix` 时才写。这一点对 enhancement PR 的适用方式与对 issue 完全相同——被拒绝的 PR 记录在此，这样同一请求就不会以新代码的形式卷土重来。

当某物因为**已经实现**而被关为 `wontfix` 时，**不要**写到这里。那是一个已构建的功能，而非被拒绝的功能；记录它会用假拒绝污染去重检查。这种情况下，关闭评论应指向该功能已经存在的位置。

流程：

1. 维护者判定某个功能请求超出范围
2. 检查是否已存在匹配的 `.out-of-scope/` 文件
3. 如果有：把新 issue 追加到 "Prior requests" 列表
4. 如果没有：用概念名、决定、理由和第一条 prior request 创建一个新文件
5. 在 issue 上发一条评论，解释这个决定并提及该 `.out-of-scope/` 文件
6. 用 `wontfix` 标签关闭该 issue

## 更新或移除 out-of-scope 文件

如果维护者改变了对某个先前被拒绝概念的看法：

- 删除该 `.out-of-scope/` 文件
- 技能不需要重新打开旧 issue——它们是历史记录
- 触发这次重新考虑的那个新 issue 走正常分诊流程
