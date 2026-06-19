---
name: design-an-interface
description: 使用并行子代理为一个模块生成多个截然不同的接口设计。当用户想要设计 API、探索接口选项、对比模块形态，或提到 "design it twice" 时使用。
---

# 设计接口

基于《A Philosophy of Software Design》中的 "Design It Twice"：你的第一个想法不太可能是最好的。先生成多个截然不同的设计，然后再对比。

## 工作流

### 1. 收集需求

设计之前，先理解：

- [ ] 这个模块解决什么问题？
- [ ] 谁是调用者？（其他模块、外部用户、测试）
- [ ] 关键操作有哪些？
- [ ] 有什么约束？（性能、兼容性、既有模式）
- [ ] 哪些应当隐藏在内部，哪些应当对外暴露？

询问："这个模块需要做什么？谁会使用它？"

### 2. 生成设计（并行子代理）

使用 Task 工具同时派生 3 个以上子代理。每个都必须产出一种**截然不同**的方案。

```
Prompt template for each sub-agent:

Design an interface for: [module description]

Requirements: [gathered requirements]

Constraints for this design: [assign a different constraint to each agent]
- Agent 1: "Minimize method count - aim for 1-3 methods max"
- Agent 2: "Maximize flexibility - support many use cases"
- Agent 3: "Optimize for the most common case"
- Agent 4: "Take inspiration from [specific paradigm/library]"

Output format:
1. Interface signature (types/methods)
2. Usage example (how caller uses it)
3. What this design hides internally
4. Trade-offs of this approach
```

### 3. 展示设计

展示每个设计，包含：

1. **接口签名** —— 类型、方法、参数
2. **使用示例** —— 调用者实际如何在实践中使用它
3. **它隐藏了什么** —— 保留在内部的复杂度

依次展示各个设计，让用户在对比之前能逐一吸收每种方案。

### 4. 对比设计

展示完所有设计后，从以下维度对比：

- **接口简洁性**：方法更少、参数更简单
- **通用 vs 专用**：灵活性 vs 聚焦
- **实现效率**：这种形态能否实现高效的内部实现？
- **深度**：小接口隐藏可观的复杂度（好）vs 大接口配薄实现（差）
- **正确使用的难易程度** vs **误用的难易程度**

用散文而非表格来讨论权衡。突出各设计分歧最大的地方。

### 5. 综合

最好的设计往往综合了多个选项中的洞见。询问：

- "哪个设计最契合你的主要用例？"
- "其他设计里有哪些元素值得吸纳进来？"

## 评估标准

出自《A Philosophy of Software Design》：

**接口简洁性**：方法更少、参数更简单 = 更易于学习和正确使用。

**通用性**：无需改动即可应对未来的用例。但要警惕过度泛化。

**实现效率**：接口形态能否支持高效的实现？还是会迫使内部实现别扭？

**深度**：小接口隐藏可观的复杂度 = 深模块（好）。大接口配薄实现 = 浅模块（避免）。

## 反模式

- 不要让子代理产出相似的设计 —— 强制要求截然不同
- 不要跳过对比 —— 价值就在于对照
- 不要实现 —— 这纯粹是关于接口形态
- 不要基于实现工作量来评估
