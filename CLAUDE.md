技能按分组目录组织，位于 `skills/` 下：

- `engineering/` — 日常代码工作
- `productivity/` — 日常非代码工作流工具
- `misc/` — 留着但很少用
- `personal/` — 与我自己的环境绑定，不对外推广
- `in-progress/` — 尚未就绪、不会发布的草稿
- `deprecated/` — 不再使用

`engineering/`、`productivity/` 或 `misc/` 中的每个技能都必须在顶层 `README.md` 中有引用，并在 `.claude-plugin/plugin.json` 中有对应条目。`personal/`、`in-progress/` 和 `deprecated/` 中的技能不得出现在两者中的任何一个里。

顶层 `README.md` 中的每个技能条目都必须把技能名链接到其 `SKILL.md`。

每个分组目录都有一个 `README.md`，列出该分组中的每个技能并附一行描述，技能名链接到其 `SKILL.md`。分组目录的 `README.md` 和顶层 `README.md` 把条目分为「用户触发」和「模型触发」两组。

每个 `SKILL.md` 要么是用户触发（`disable-model-invocation: true`，只有人类可达），要么是模型触发（模型或用户均可达）。完整定义、描述约定，以及为什么用户触发的技能可以调用模型触发的技能、却永远无法调用另一个用户触发的技能，参见 [docs/invocation.md](./docs/invocation.md)。
