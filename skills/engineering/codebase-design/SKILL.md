---
name: codebase-design
description: 设计深模块时共享的词汇。当用户想设计或改进某个模块的接口、寻找加深机会、决定接缝放在哪里、让代码更可测试或更便于 AI 导航，或当另一个技能需要这套深模块词汇时使用。
---

# 代码库设计

设计 **深模块**：大量行为藏在小接口背后，置于干净的接缝处，可通过那个接口测试。无论在何处设计或重构代码，都使用这套语言和这些原则。目标是给调用方杠杆、给维护者局部性、给所有人可测试性。

## 术语表

精确地使用这些术语 —— 不要用「component」「service」「API」或「boundary」替换。一致的语言正是要害所在。

**Module** —— 任何拥有接口和实现的东西。刻意做到与规模无关：一个函数、类、包，或一个横跨层级的切片。_避免_：unit、component、service。

**Interface** —— 调用方为正确使用模块所必须知道的一切：类型签名，但也包括不变量、顺序约束、错误模式、必需的配置和性能特征。_避免_：API、signature（太窄 —— 它们只指类型层面的表面）。

**Implementation** —— 模块内部是什么，它那一身代码。区别于 **Adapter**：一个东西可以是小 adapter 配大实现（Postgres 仓储），也可以是大 adapter 配小实现（内存 fake）。当话题是接缝时选用「adapter」；否则选用「implementation」。

**Depth** —— 接口处的杠杆：调用方（或测试）每学习一个单位的接口，能驱动多少行为。当大量行为藏在小接口背后时，模块是 **深** 的；当接口几乎和实现一样复杂时，是 **浅** 的。

**Seam** _(Michael Feathers)_ —— 一个你无需在该处编辑就能改变行为的地方；模块接口所在的 *位置*。接缝放在哪里是它自己的设计决策，与放什么在它背后是两回事。_避免_：boundary（与 DDD 的限界上下文含义重叠）。

**Adapter** —— 在接缝处满足某个接口的具体东西。描述的是 *角色*（它填的是哪个槽位），而非实质（里面是什么）。

**Leverage** —— 调用方从 depth 中得到的东西：每学习一个单位的接口换得更多能力。一份实现在 N 个调用点和 M 个测试上反复回本。

**Locality** —— 维护者从 depth 中得到的东西：变更、bug、知识和验证集中在一处，而非散布到各个调用方。改一次，处处修好。

## 深模块 vs 浅模块

**深模块** = 小接口 + 大量实现：

```
┌─────────────────────┐
│   Small Interface   │  ← Few methods, simple params
├─────────────────────┤
│                     │
│  Deep Implementation│  ← Complex logic hidden
│                     │
└─────────────────────┘
```

**浅模块** = 大接口 + 少量实现（避免）：

```
┌─────────────────────────────────┐
│       Large Interface           │  ← Many methods, complex params
├─────────────────────────────────┤
│  Thin Implementation            │  ← Just passes through
└─────────────────────────────────┘
```

设计接口时，自问：

- 我能减少方法的数量吗？
- 我能简化参数吗？
- 我能把更多复杂性藏在里面吗？

## 原则

- **Depth 是接口的属性，而非实现的属性。** 一个深模块的内部可以由小的、可 mock 的、可替换的部件组成 —— 只是它们不属于接口的一部分。一个模块既可以有 **内部接缝**（对其实现私有，供它自己的测试使用），也可以有位于其接口处的 **外部接缝**。
- **删除测试。** 设想删掉这个模块。如果复杂性随之消失，它就是个直通件。如果复杂性在 N 个调用方处重新冒出来，那它就在挣自己的口粮。
- **接口就是测试面。** 调用方和测试跨越同一道接缝。如果你想测试 *越过* 接口的东西，那这个模块的形态多半不对。
- **一个 adapter 意味着假想的接缝。两个 adapter 才意味着真实的接缝。** 除非确实有东西在接缝两侧发生变化，否则不要引入接缝。

## 为可测试性而设计

好的接口让测试变得自然：

1. **接收依赖，不要创建依赖。**

   ```typescript
   // Testable
   function processOrder(order, paymentGateway) {}

   // Hard to test
   function processOrder(order) {
     const gateway = new StripeGateway();
   }
   ```

2. **返回结果，不要产生副作用。**

   ```typescript
   // Testable
   function calculateDiscount(cart): Discount {}

   // Hard to test
   function applyDiscount(cart): void {
     cart.total -= discount;
   }
   ```

3. **小表面积。** 方法越少 = 需要的测试越少。参数越少 = 测试搭建越简单。

## 关系

- 一个 **Module** 恰好有一个 **Interface**（它呈现给调用方和测试的表面）。
- **Depth** 是 **Module** 的属性，相对其 **Interface** 来衡量。
- **Seam** 是 **Module** 的 **Interface** 所在之处。
- **Adapter** 坐落在 **Seam** 处并满足 **Interface**。
- **Depth** 为调用方产生 **Leverage**，为维护者产生 **Locality**。

## 被否决的提法

- **把 depth 当作实现行数与接口行数之比**（Ousterhout）：这会奖励往实现里塞水分。我们改用「depth 即杠杆」。
- **把「interface」当作 TypeScript 的 `interface` 关键字或一个类的公开方法**：太窄 —— 这里的 interface 包含调用方必须知道的每一个事实。
- **「Boundary」**：与 DDD 的限界上下文含义重叠。改说 **seam** 或 **interface**。

## 更进一步

- **在已知依赖的情况下加深一簇模块** —— 见 [DEEPENING.md](DEEPENING.md)：依赖分类、接缝规约，以及「替换而非分层」的测试。
- **探索备选接口** —— 见 [DESIGN-IT-TWICE.md](DESIGN-IT-TWICE.md)：拉起并行子代理，用几种截然不同的方式设计接口，再从 depth、locality 和接缝位置上对比。
