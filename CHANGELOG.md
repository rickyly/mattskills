# mattpocock-skills

## 1.0.1

### Patch Changes

- [`d20ee26`](https://github.com/mattpocock/skills/commit/d20ee2684e2a9442698ac3c1e0f2c5b68c4cf296) Thanks [@mattpocock](https://github.com/mattpocock)! - 让 **`teach`** 技能以复用为先。课程现在由 `./assets/` 中可复用的**组件**搭建而成——样式表、测验小组件、模拟器、图表辅助工具。复用是默认做法：agent 在编写课程之前先读取 `./assets/`，从已有的东西出发来搭建，并把任何新的、可复用的东西抽取成一个组件，而不是内联进去。

## 1.0.0

### Major Changes

- [`47bde84`](https://github.com/mattpocock/skills/commit/47bde84da032afb2e5058f997f3bbca47d321dbd) Thanks [@mattpocock](https://github.com/mattpocock)! - 新增 **`ask-matt`** 技能——一个用户触发的路由器，为你的处境指向合适的技能或流程。

  **Breaking:** `ask-matt` 在本仓库其他用户触发的技能之上做路由，因此它要求这些技能都已安装。

- [`47bde84`](https://github.com/mattpocock/skills/commit/47bde84da032afb2e5058f997f3bbca47d321dbd) Thanks [@mattpocock](https://github.com/mattpocock)! - 新增共享的设计类技能，并把现有技能重新接入它们。

  - 新增 **`codebase-design`** 技能——深模块词汇表（module、interface、depth、seam、adapter）以及把大量行为藏在小接口背后的原则。先前存放在 `improve-codebase-architecture/LANGUAGE.md` 中的这套语言，现在存放在这里，并做了泛化以便在多个技能间复用。
  - 新增 **`domain-modeling`** 技能——主动构建并打磨项目的领域模型，对照术语表对术语做压力测试，并保持 `CONTEXT.md` 和 ADR 最新。
  - `improve-codebase-architecture` 现在从 `/codebase-design` 汲取其架构词汇，从 `/domain-modeling` 汲取其领域模型。
  - `tdd` 现在依靠 `/codebase-design` 提供接口设计指导——它内联的 `deep-modules.md` / `interface-design.md` 笔记已被移除，改用这个共享技能。
  - `grill-with-docs` 现在通过 `/domain-modeling` 内联构建领域模型。

  **Breaking:** 这些技能现在依赖新的 `codebase-design` / `domain-modeling` 技能，因此你也必须安装它们。

- [`47bde84`](https://github.com/mattpocock/skills/commit/47bde84da032afb2e5058f997f3bbca47d321dbd) Thanks [@mattpocock](https://github.com/mattpocock)! - 移除 **`caveman`** 和 **`zoom-out`** 技能。

  - `caveman` 是我当时在测试的另一个技能的副本，本就不该公开。
  - `zoom-out` 在实践中没被用到，因此已从仓库中移除。

  **Breaking:** 两个技能都已被移除。

- [`47bde84`](https://github.com/mattpocock/skills/commit/47bde84da032afb2e5058f997f3bbca47d321dbd) Thanks [@mattpocock](https://github.com/mattpocock)! - 把 **`diagnose`** 技能重命名为 **`diagnosing-bugs`**。

  **Breaking:** 用 `/diagnosing-bugs` 来调用它——旧的 `/diagnose` 名称不再存在。

- [`47bde84`](https://github.com/mattpocock/skills/commit/47bde84da032afb2e5058f997f3bbca47d321dbd) Thanks [@mattpocock](https://github.com/mattpocock)! - 用 **`writing-great-skills`** 替换 **`write-a-skill`**。

  - 移除了 `write-a-skill`。
  - 新增了 `writing-great-skills`（及其 `GLOSSARY.md`）——一份关于如何写好和编辑好技能的参考：让技能可预测的那套词汇和原则，把无用操作（no-op）追查到句子级别。
  - 把 `grilling` 暴露为一个模型触发的技能——它是 `grill-me` 和 `grill-with-docs` 背后那个可复用的访谈循环。

  **Breaking:** `write-a-skill` 已被移除；请改用 `writing-great-skills`。

### Minor Changes

- [`47bde84`](https://github.com/mattpocock/skills/commit/47bde84da032afb2e5058f997f3bbca47d321dbd) Thanks [@mattpocock](https://github.com/mattpocock)! - 新增 **`resolving-merge-conflicts`** 技能——一个用于解决进行中的 git 合并或 rebase 冲突的循环。独立运行，不依赖其他技能。

- [`47bde84`](https://github.com/mattpocock/skills/commit/47bde84da032afb2e5058f997f3bbca47d321dbd) Thanks [@mattpocock](https://github.com/mattpocock)! - 把技能分类法从 **Commands / Skills** 在文档中全面重命名为 **User-invoked / Model-invoked**，并新增 `docs/invocation.md` 来定义这一区分：用户触发的技能只有在你输入它们时才可达，存在的意义是做编排；模型触发的技能在任务契合时也可被自动触达。一个用户触发的技能可以触发模型触发的技能，但绝不能触发另一个用户触发的技能。

### Patch Changes

- [`47bde84`](https://github.com/mattpocock/skills/commit/47bde84da032afb2e5058f997f3bbca47d321dbd) Thanks [@mattpocock](https://github.com/mattpocock)! - 收紧 **`review`** 技能：快速失败的 ref 检查、单一来源的规则，以及砍掉无用操作（no-op）。
