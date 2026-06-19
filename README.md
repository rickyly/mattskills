<p>
  <a href="https://www.aihero.dev/s/skills-newsletter">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skills-repo-dark_2x.png">
      <source media="(prefers-color-scheme: light)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png">
      <img alt="Skills" src="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png" width="369">
    </picture>
  </a>
</p>

# 面向真正工程师的技能

[![skills.sh](https://skills.sh/b/mattpocock/skills)](https://skills.sh/mattpocock/skills)

我每天用来做真正工程的代理技能——而不是 vibe coding。

开发真正的应用很难。GSD、BMAD、Spec-Kit 这类方法试图通过接管整个流程来提供帮助。但在这样做的同时，它们夺走了你的控制权，让流程中的 bug 难以解决。

这些技能被设计得小巧、易于改造、可组合。它们能与任何模型配合工作。它们建立在数十年工程经验之上。尽管折腾它们。把它们变成你自己的。享受其中。

如果你想跟进这些技能的变动，以及我创建的任何新技能，可以加入我的简报，与另外约 6 万名开发者同行：

[Sign Up To The Newsletter](https://www.aihero.dev/s/skills-newsletter)

## 快速开始（30 秒安装）

1. 运行 skills.sh 安装器：

```bash
npx skills@latest add mattpocock/skills
```

2. 挑选你想要的技能，以及你想把它们装到哪些编码代理上。**务必选中 `/setup-matt-pocock-skills`**。

3. 在你的代理中运行 `/setup-matt-pocock-skills`。它会：
   - 询问你想用哪个 issue tracker（GitHub、Linear 或本地文件）
   - 询问你在 triage 时给 ticket 打哪些标签（`/triage` 使用标签）
   - 询问你想把我们创建的文档保存到哪里

4. 搞定——你准备就绪了。

## 这些技能为何存在

我构建这些技能，是为了修复我在 Claude Code、Codex 以及其他编码代理身上看到的常见失败模式。

### #1：代理没做我想要的

> “No-one knows exactly what they want”
>
> David Thomas & Andrew Hunt，[The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题所在**。软件开发中最常见的失败模式是错位。你以为开发者知道你想要什么。然后你看到他们做出来的东西——这才意识到他们根本没理解你。

在 AI 时代这一点别无二致。你和代理之间存在沟通鸿沟。修复它的办法是一场 **grilling session**——让代理就你正在构建的东西向你提出详细问题。

**修复办法**是使用：

- [`/grill-me`](./skills/productivity/grill-me/SKILL.md) — 用于非代码场景
- [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) — 与 [`/grill-me`](./skills/productivity/grill-me/SKILL.md) 相同，但增添了更多好东西（见下文）

这些是我最受欢迎的技能。它们帮你在动手前与代理对齐，并就你正在做的改动深入思考。每次想做改动时都用一用它们。

### #2：代理太啰嗦了

> With a ubiquitous language, conversations among developers and expressions of the code are all derived from the same domain model.
>
> Eric Evans，[Domain-Driven-Design](https://www.amazon.co.uk/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)

**问题所在**：在项目伊始，开发者和他们为之构建软件的人（领域专家）通常说着不同的语言。

我对自己的代理也感受到了同样的张力。代理通常被丢进一个项目，被要求边做边搞懂那些行话。于是它们用 20 个词去说本该 1 个词就能说清的事。

**修复办法**是一门共享语言。它是一份帮助代理解码项目中所用行话的文档。

<details>
<summary>
示例
</summary>

这是一个来自我 `course-video-manager` 仓库的 [`CONTEXT.md`](https://github.com/mattpocock/course-video-manager/blob/076a5a7a182db0fe1e62971dd7a68bcadf010f1c/CONTEXT.md) 示例。哪一个更易读？

- **之前**：“There's a problem when a lesson inside a section of a course is made 'real' (i.e. given a spot in the file system)”
- **之后**：“There's a problem with the materialization cascade”

这种凝练会在一次又一次的会话中持续回报。

</details>

这内置于 [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md)。它是一场 grilling session，但能帮你与 AI 建立共享语言，并把难以解释的决策记录到 ADR 中。

这有多强大，很难言说。它或许是本仓库里最酷的一项技术。试试看，自见分晓。

> [!TIP]
> 共享语言除了减少啰嗦之外还有许多好处：
>
> - **变量、函数和文件命名一致**，使用共享语言
> - 因此，**代码库更易于导航**，对代理而言如此
> - 代理还会**在思考上花费更少的 token**，因为它拥有一门更凝练的语言

### #3：代码不工作

> “Always take small, deliberate steps. The rate of feedback is your speed limit. Never take on a task that’s too big.”
>
> David Thomas & Andrew Hunt，[The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题所在**：假设你和代理已就构建什么达成一致。当代理_仍然_产出垃圾时会发生什么？

是时候审视你的反馈循环了。如果没有关于它所产出代码实际运行情况的反馈，代理就会盲飞。

**修复办法**：你需要那一整套常见的反馈循环：静态类型、浏览器访问和自动化测试。

对于自动化测试，红-绿-重构循环至关重要。在这个循环里，代理先写一个失败的测试，然后修复这个测试。这有助于给代理提供一致水平的反馈，从而产出好得多的代码。

我构建了一个 **[`/tdd`](./skills/engineering/tdd/SKILL.md) 技能**，你可以把它嵌入任何项目。它鼓励红-绿-重构，并就好测试与坏测试的区别给代理大量指引。

对于调试，我还构建了一个 **[`/diagnosing-bugs`](./skills/engineering/diagnosing-bugs/SKILL.md)** 技能，把最佳调试实践包裹进一个简单的循环。

### #4：我们建了一个烂泥球

> “Invest in the design of the system _every day_.”
>
> Kent Beck，[Extreme Programming Explained](https://www.amazon.co.uk/Extreme-Programming-Explained-Embrace-Change/dp/0321278658)

> “The best modules are deep. They allow a lot of functionality to be accessed through a simple interface.”
>
> John Ousterhout，[A Philosophy Of Software Design](https://www.amazon.co.uk/Philosophy-Software-Design-2nd/dp/173210221X)

**问题所在**：大多数用代理构建的应用都复杂且难以更改。由于代理能极大加快编码速度，它们也加速了软件熵增。代码库以前所未有的速度变得更复杂。

**修复办法**是一种面向 AI 驱动开发的全新激进方式：在乎代码的设计。

这内置于这些技能的每一层：

- [`/to-prd`](./skills/engineering/to-prd/SKILL.md) 在创建 PRD 之前，会就你正在触碰哪些模块对你发问

而关键在于，[`/improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.md) 能帮你拯救一个已经变成烂泥球的代码库。我建议每隔几天就在你的代码库上运行一次。

### 小结

软件工程的基本功比以往任何时候都更重要。这些技能是我把这些基本功凝练成可重复实践的最大努力，旨在帮你交付职业生涯中最好的应用。享受其中。

## 参考

它们沿一条轴划分——谁可以触发它们。**用户触发**的技能只有在你键入它们时才可达（如 `/grill-me`）；它们的职责是编排。**模型触发**的技能可由你触发，_或_由代理在任务契合时自动达到；它们承载可复用的纪律。用户触发的技能可以调用模型触发的技能，却永远无法调用另一个用户触发的技能。

### 工程

我每天用于代码工作的技能。

**用户触发**

- **[ask-matt](./skills/engineering/ask-matt/SKILL.md)** — 询问哪个技能或流程契合你的处境。本仓库中用户触发技能之上的一个路由器。
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)** — 一场同时构建项目领域模型的 grilling session，打磨术语并就地更新 `CONTEXT.md` 和 ADR。
- **[triage](./skills/engineering/triage/SKILL.md)** — 让 issue 在 triage 角色的状态机中流转。
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)** — 扫描代码库寻找深化机会，以可视化 HTML 报告呈现，然后就你挑中的那一个进行 grill。
- **[setup-matt-pocock-skills](./skills/engineering/setup-matt-pocock-skills/SKILL.md)** — 为工程技能配置本仓库（issue tracker、triage 标签、领域文档布局）。在使用其他工程技能之前，每个仓库运行一次。
- **[to-issues](./skills/engineering/to-issues/SKILL.md)** — 用垂直切片把任何计划、规格或 PRD 拆成可独立认领的 issue。
- **[to-prd](./skills/engineering/to-prd/SKILL.md)** — 把当前对话变成一份 PRD 并发布到 issue tracker。没有访谈——只是综合你已经讨论过的内容。
- **[prototype](./skills/engineering/prototype/SKILL.md)** — 构建一个一次性原型来充实一个设计——要么是用于状态/业务逻辑问题的可运行终端应用，要么是几个可从单条路由切换的、彻底不同的 UI 变体。

**模型触发**

- **[diagnosing-bugs](./skills/engineering/diagnosing-bugs/SKILL.md)** — 针对疑难 bug 和性能回归的严谨诊断循环：复现 → 最小化 → 假设 → 插桩 → 修复 → 回归测试。
- **[tdd](./skills/engineering/tdd/SKILL.md)** — 带红-绿-重构循环的测试驱动开发。一次一个垂直切片地构建特性或修复 bug。
- **[domain-modeling](./skills/engineering/domain-modeling/SKILL.md)** — 主动构建并打磨项目的领域模型——对照术语表挑战术语、用边界场景压力测试，并就地更新 `CONTEXT.md` 和 ADR。
- **[codebase-design](./skills/engineering/codebase-design/SKILL.md)** — 设计深模块的共享纪律与词汇：在一个干净的接缝处，以一个小接口背后承载大量行为，并可通过该接口测试。

### 生产力

通用工作流工具，不限于代码。

**用户触发**

- **[grill-me](./skills/productivity/grill-me/SKILL.md)** — 就一个计划或设计被不留情面地访谈，直到决策树的每个分支都得到解决。
- **[handoff](./skills/productivity/handoff/SKILL.md)** — 把当前对话压缩成一份交接文档，好让另一个代理继续这项工作。
- **[teach](./skills/productivity/teach/SKILL.md)** — 跨多次会话教用户一项新技能或概念，把当前目录用作一个有状态的教学工作区。
- **[writing-great-skills](./skills/productivity/writing-great-skills/SKILL.md)** — 把技能写好、编辑好的参考：让技能可预测的那些词汇与原则。

**模型触发**

- **[grilling](./skills/productivity/grilling/SKILL.md)** — 就一个计划或设计不留情面地访谈用户，直到决策树的每个分支都得到解决。`grill-me` 和 `grill-with-docs` 背后那个可复用的循环。

### 杂项

我留着但很少用的工具。

- **[git-guardrails-claude-code](./skills/misc/git-guardrails-claude-code/SKILL.md)** — 设置 Claude Code 钩子，在危险的 git 命令（push、reset --hard、clean 等）执行前将其拦截。
- **[migrate-to-shoehorn](./skills/misc/migrate-to-shoehorn/SKILL.md)** — 把测试文件从 `as` 类型断言迁移到 @total-typescript/shoehorn。
- **[scaffold-exercises](./skills/misc/scaffold-exercises/SKILL.md)** — 创建带章节、习题、解答和讲解的习题目录结构。
- **[setup-pre-commit](./skills/misc/setup-pre-commit/SKILL.md)** — 设置带 lint-staged、Prettier、类型检查和测试的 Husky pre-commit 钩子。
