---
name: to-issues
description: 用 tracer-bullet 垂直切片，把计划、规格或 PRD 拆成可在项目 issue 跟踪器上独立认领的 issue。
disable-model-invocation: true
---

# 拆分为 Issue

用垂直切片（tracer bullet）把计划拆成可独立认领的 issue。

issue 跟踪器和 triage 标签词汇应当已经提供给你——如果没有，运行 `/setup-matt-pocock-skills`。

## 流程

### 1. 收集上下文

从对话上下文中已有的内容入手。如果用户把一个 issue 引用（issue 编号、URL 或路径）作为参数传入，从 issue 跟踪器获取它，并读取其完整正文和评论。

### 2. 探索代码库（可选）

如果你还没有探索过代码库，现在就去探索，以理解代码的当前状态。issue 标题和描述应当使用项目领域术语表的词汇，并尊重你所改动区域内的 ADR。

寻找对代码做预重构（prefactor）以让实现更容易的机会。“先让改动变容易，再做那个容易的改动。”

### 3. 起草垂直切片

把计划拆成 **tracer bullet** issue。每个 issue 都是一个薄薄的垂直切片，端到端贯穿 **所有** 集成层，而 **不是** 单一层的水平切片。

<vertical-slice-rules>

- 每个切片交付一条狭窄但 **完整** 的路径，贯穿每一层（schema、API、UI、测试）
- 一个完成的切片本身可演示或可验证
- 任何预重构都应当先做

</vertical-slice-rules>

### 4. 向用户提问

把提议的拆分方案以编号列表呈现。对每个切片，展示：

- **Title**：简短的描述性名称
- **Blocked by**：哪些其他切片（如果有）必须先完成
- **User stories covered**：这个切片处理了哪些用户故事（如果源材料里有的话）

询问用户：

- 粒度感觉合适吗？（太粗 / 太细）
- 依赖关系正确吗？
- 是否应该合并或进一步拆分某些切片？

反复迭代，直到用户批准拆分方案。

### 5. 把 issue 发布到 issue 跟踪器

对每个获批的切片，向 issue 跟踪器发布一个新 issue。使用下面的 issue 正文模板。这些 issue 被视为已为 AFK 代理准备就绪，因此除非另有指示，发布时打上正确的 triage 标签。

按依赖顺序发布 issue（先发阻塞者），这样你就能在 “Blocked by” 字段中引用真实的 issue 标识符。

<issue-template>
## Parent

对 issue 跟踪器上父 issue 的引用（如果源材料是一个已有的 issue，否则省略本节）。

## What to build

对这个垂直切片的简明描述。描述端到端的行为，而非逐层的实现。

避免具体的文件路径或代码片段——它们很快就会过时。例外：如果某个原型产出的片段比散文更精确地编码了一个决策（状态机、reducer、schema、类型形状），就把它内联在这里，并简要注明它来自一个原型。修剪到富含决策的部分——不是一个可运行的演示，只是关键的几处。

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Blocked by

- 对阻塞工单的引用（如果有）

否则填 “None - can start immediately”，如果没有阻塞者。

</issue-template>

不要关闭或修改任何父 issue。
