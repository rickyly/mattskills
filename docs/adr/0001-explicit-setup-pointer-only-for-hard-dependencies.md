# 仅对硬依赖给出显式的 `/setup-matt-pocock-skills` 指引

工程类技能依赖由 `/setup-matt-pocock-skills` 播种的每仓库配置（issue tracker、triage 标签词汇、领域文档布局）。有些技能离开该配置就无法有意义地工作——它们必须发布到某个特定的 issue tracker，或施加某个特定的标签字符串。另一些技能只是用它来打磨输出（词汇、ADR 意识），缺了它也能平稳降级。

我们把这些技能分为**硬依赖**和**软依赖**两类：

- **硬依赖**（`to-issues`、`to-prd`、`triage`）——包含一句显式提示：_“……应该已经提供给你——如果没有，运行 `/setup-matt-pocock-skills`。”_ 缺了这层映射，输出就是错的，而不仅是模糊。
- **软依赖**（`diagnose`、`tdd`、`improve-codebase-architecture`）——仅以含糊的散文引用“项目的领域术语表”和“你所触碰区域的 ADR”。即便文档不在，技能照样能用，只是输出不那么精准。

这种划分让软依赖技能保持 token 轻量，并避免把这条 setup 指引盲目照搬到它并不起承载作用的地方。
