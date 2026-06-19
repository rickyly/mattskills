---
name: to-prd
description: 把当前对话变成一份 PRD 并发布到项目 issue 跟踪器——无需访谈，只综合你们已经讨论过的内容。
disable-model-invocation: true
---

这个技能接收当前对话上下文和对代码库的理解，产出一份 PRD。不要访谈用户——只综合你已经知道的内容。

issue 跟踪器和 triage 标签词汇应当已经提供给你——如果没有，运行 `/setup-matt-pocock-skills`。

## 流程

1. 探索仓库以理解代码库的当前状态（如果你还没这么做的话）。在整份 PRD 中使用项目领域术语表的词汇，并尊重你所改动区域内的任何 ADR。

2. 勾勒出你打算用来测试该功能的接缝（seam）。应当优先使用已有的接缝，而非新建。使用尽可能高层的接缝。如果需要新接缝，在你能达到的最高点提出它们。代码库中跨越的接缝越少越好——理想数量是一个。

   与用户确认这些接缝是否符合他们的预期。

3. 用下面的模板写 PRD，然后把它发布到项目 issue 跟踪器。打上 `ready-for-agent` triage 标签——无需额外 triage。

<prd-template>

## Problem Statement

用户正面临的问题，从用户的视角出发。

## Solution

问题的解决方案，从用户的视角出发。

## User Stories

一份 **长长的** 编号用户故事列表。每条用户故事应采用如下格式：

1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending
</user-story-example>

这份用户故事列表应当极其详尽，覆盖该功能的所有方面。

## Implementation Decisions

一份已做出的实现决策列表。可以包括：

- 将要构建 / 修改的模块
- 这些模块中将要修改的接口
- 来自开发者的技术澄清
- 架构决策
- schema 变更
- API 契约
- 具体的交互

不要包含具体的文件路径或代码片段。它们可能很快就会过时。

例外：如果某个原型产出的片段比散文更精确地编码了一个决策（状态机、reducer、schema、类型形状），就把它内联在相关决策中，并简要注明它来自一个原型。修剪到富含决策的部分——不是一个可运行的演示，只是关键的几处。

## Testing Decisions

一份已做出的测试决策列表。包括：

- 描述什么构成一个好测试（只测试外部行为，不测试实现细节）
- 哪些模块将被测试
- 测试的前人经验（即代码库中类似类型的测试）

## Out of Scope

描述本 PRD 范围之外的事项。

## Further Notes

关于该功能的任何补充说明。

</prd-template>
