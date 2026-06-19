# 领域文档

工程类技能在探查代码库时，应如何消费本仓库的领域文档。

## 探查前，先读这些

- 仓库根目录的 **`CONTEXT.md`**，或
- 仓库根目录的 **`CONTEXT-MAP.md`**（如果存在）——它为每个上下文指向一个 `CONTEXT.md`。读取与当前主题相关的每一个。
- **`docs/adr/`**——读取涉及你即将着手领域的那些 ADR。在多上下文仓库中，还要检查 `src/<context>/docs/adr/` 中按上下文限定的决策。

如果其中任何文件不存在，**静默继续**。不要标记它们的缺失；不要在一开始就建议创建它们。`/domain-modeling` 技能（经由 `/grill-with-docs` 和 `/improve-codebase-architecture` 触达）会在术语或决策真正被解决时惰性地创建它们。

## 文件结构

单上下文仓库（大多数仓库）：

```
/
├── CONTEXT.md
├── docs/adr/
│   ├── 0001-event-sourced-orders.md
│   └── 0002-postgres-for-write-model.md
└── src/
```

多上下文仓库（根目录存在 `CONTEXT-MAP.md`）：

```
/
├── CONTEXT-MAP.md
├── docs/adr/                          ← system-wide decisions
└── src/
    ├── ordering/
    │   ├── CONTEXT.md
    │   └── docs/adr/                  ← context-specific decisions
    └── billing/
        ├── CONTEXT.md
        └── docs/adr/
```

## 使用术语表的词汇

当你的输出命名一个领域概念时（在 issue 标题、重构提案、假设、测试名中），使用 `CONTEXT.md` 中定义的术语。不要漂移到术语表明确避免的同义词。

如果你需要的概念还不在术语表中，这是一个信号——要么你在发明项目并不使用的语言（重新考虑），要么存在一个真实的缺口（为 `/domain-modeling` 记下它）。

## 标记 ADR 冲突

如果你的输出与某个现有 ADR 相矛盾，明确地把它摆出来，而不是悄悄覆盖：

> _Contradicts ADR-0007 (event-sourced orders) — but worth reopening because…_
