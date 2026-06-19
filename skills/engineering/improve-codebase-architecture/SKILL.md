---
name: improve-codebase-architecture
description: 扫描代码库寻找深化机会，以可视化 HTML 报告呈现，然后就你选中的那一项逐一拷问。
disable-model-invocation: true
---

# 改进代码库架构

暴露架构上的摩擦，并提出**深化机会**——把浅模块变成深模块的重构。目标是可测试性与 AI 可导航性。

这条命令_借助_项目的领域模型，并建立在一套共享的设计词汇之上：

- 运行 `/codebase-design` 技能，获取架构词汇（**module**、**interface**、**depth**、**seam**、**adapter**、**leverage**、**locality**）及其原则（删除测试、「the interface is the test surface」、「one adapter = hypothetical seam, two = real」）。在每条建议中都精确使用这些术语——不要漂移成「component」「service」「API」或「boundary」。
- `CONTEXT.md` 中的领域语言为好的 seam 命名；`docs/adr/` 中的 ADR 记录了这条命令不应重新争论的决策。

## 流程

### 1. 探索

先读项目的领域术语表（`CONTEXT.md`），以及你要触碰的区域内的任何 ADR。

然后用 Agent 工具配合 `subagent_type=Explore` 走查代码库。不要套用僵化的启发式规则——有机地探索，并记下你在哪里感到摩擦：

- 在哪里理解一个概念需要在许多小模块之间来回跳转？
- 哪里的模块是**浅**的——interface 几乎和 implementation 一样复杂？
- 哪里的纯函数只是为了可测试性而被抽取出来，但真正的 bug 藏在它们如何被调用之中（没有 **locality**）？
- 哪里紧耦合的模块跨越各自的 seam 泄漏？
- 代码库的哪些部分未经测试，或者通过当前 interface 难以测试？

对任何你怀疑是浅的东西套用**删除测试**：删掉它会让复杂度集中，还是只是把它挪个地方？「会，集中」就是你想要的信号。

### 2. 以 HTML 报告呈现候选项

把一个独立完整的 HTML 文件写到操作系统的临时目录，这样不会有东西落进仓库。从 `$TMPDIR` 解析临时目录，回退到 `/tmp`（在 Windows 上回退到 `%TEMP%`），并写入 `<tmpdir>/architecture-review-<timestamp>.html`，使每次运行都得到一个全新文件。为用户打开它——Linux 上用 `xdg-open <path>`，macOS 上用 `open <path>`，Windows 上用 `start <path>`——并告诉他们绝对路径。

报告用 **Tailwind via CDN** 做布局和样式，用 **Mermaid via CDN** 画那些图/流程/序列能可靠传达结构的示意图。把 Mermaid 与手工打造的 CSS/SVG 视觉效果混用——当关系是图形状（调用图、依赖、序列）时用 Mermaid，当你想要更具编排性的东西（质量图、剖面图、坍缩动画）时用手工搭建的 div/SVG。每个候选项都配一张**之前/之后的可视化图**。要视觉化。

为每个候选项渲染一张卡片，包含：

- **Files**——涉及哪些文件/模块
- **Problem**——为什么当前架构在制造摩擦
- **Solution**——用平实的英文描述会改变什么
- **Benefits**——用 locality 和 leverage 来解释，以及测试会如何改善
- **Before / After diagram**——并排、自定义绘制，说明浅之处与深化方式
- **Recommendation strength**——`Strong`、`Worth exploring`、`Speculative` 三者之一，渲染为徽章

在报告结尾加一个 **Top recommendation** 小节：你会先着手哪个候选项，以及为什么。

**领域方面用 CONTEXT.md 的词汇，架构方面用 `/codebase-design` 的词汇。** 如果 `CONTEXT.md` 定义了「Order」，就说「the Order intake module」——而不是「the FooBarHandler」，也不是「the Order service」。

**ADR 冲突**：如果某个候选项与现有 ADR 相抵触，只在摩擦真实到足以让人重新审视该 ADR 时才暴露它。在卡片中清楚标注（例如一个警告标注：_"contradicts ADR-0007 — but worth reopening because…"_）。不要把某个 ADR 所禁止的每一种理论上的重构都列出来。

关于完整的 HTML 脚手架、示意图模式和样式指南，见 [HTML-REPORT.md](HTML-REPORT.md)。

现在还**不要**提出 interface。文件写好后，问用户：「Which of these would you like to explore?」

### 3. 拷问循环

用户选定一个候选项后，运行 `/grilling` 技能，和他们一起走查设计树——约束、依赖、深化后模块的形态、seam 背后是什么、哪些测试得以存活。

副作用在决策成形时就地发生——边走边运行 `/domain-modeling` 技能，让领域模型保持最新：

- **要用一个不在 `CONTEXT.md` 里的概念给深化后的模块命名？** 把该术语加进 `CONTEXT.md`。如果文件不存在就惰性创建它。
- **在对话中把一个模糊的术语磨清晰了？** 当场更新 `CONTEXT.md`。
- **用户以一个承重的理由否决了某个候选项？** 提议写一条 ADR，框架为：_"Want me to record this as an ADR so future architecture reviews don't re-suggest it?"_ 只在这个理由确实会被未来的探索者用来避免重复提同样建议时才提议——略过临时性的理由（「眼下不值得」）和不言自明的理由。
- **想为深化后的模块探索备选 interface？** 运行 `/codebase-design` 技能，并使用它的「设计两遍」并行子代理模式。
