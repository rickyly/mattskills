---
name: tdd
description: 测试驱动开发。当用户想以测试优先的方式构建功能或修复 bug、提到 “red-green-refactor”，或想要集成测试时使用。
---

# 测试驱动开发

## 理念

**核心原则**：测试应当通过公共接口验证行为，而非实现细节。代码可以彻底改变；测试不应随之改变。

**好的测试**是集成风格的：它们通过公共 API 走真实的代码路径。它们描述系统做 _什么_，而非 _如何_ 做。一个好的测试读起来像一份规格说明——“用户可以用有效的购物车结账”准确告诉你存在什么能力。这类测试能在重构中存活，因为它们不关心内部结构。

**坏的测试**与实现耦合。它们 mock 内部协作者、测试私有方法，或通过外部手段验证（比如直接查询数据库而不是使用接口）。警示信号：重构时测试失败，但行为并未改变。如果你重命名了一个内部函数而测试失败，那么这些测试测的是实现，而非行为。

示例见 [tests.md](tests.md)，mock 指南见 [mocking.md](mocking.md)。

## 反模式：水平切片

**不要先写完所有测试，再写所有实现。**这是 “水平切片”——把 RED 当作 “写完所有测试”，把 GREEN 当作 “写完所有代码”。

它产出的是 **垃圾测试**：

- 成批写出的测试测的是 _想象中_ 的行为，而非 _实际_ 的行为
- 你最终测的是事物的 _形状_（数据结构、函数签名），而非面向用户的行为
- 测试对真实变化变得不敏感——行为出错时它们通过，行为正常时它们失败
- 你跑到了车灯照亮的范围之外，在理解实现之前就锁定了测试结构

**正确做法**：通过 tracer bullet 做垂直切片。一个测试 → 一个实现 → 重复。每个测试都回应你从上一轮循环中学到的东西。因为代码是你刚写的，你确切知道哪些行为重要、如何验证它们。

```
WRONG (horizontal):
  RED:   test1, test2, test3, test4, test5
  GREEN: impl1, impl2, impl3, impl4, impl5

RIGHT (vertical):
  RED→GREEN: test1→impl1
  RED→GREEN: test2→impl2
  RED→GREEN: test3→impl3
  ...
```

## 工作流

### 1. 计划

探索代码库时，读 `CONTEXT.md`（如果存在），让测试名称和接口词汇与项目的领域语言保持一致，并尊重你所改动区域内的 ADR。

在写任何代码之前：

- [ ] 与用户确认需要哪些接口变更
- [ ] 与用户确认要测试哪些行为（排定优先级）
- [ ] 识别深模块（小接口、深实现）的机会——运行 `/codebase-design` 技能获取相关词汇和可测试性检查
- [ ] 列出要测试的行为（而非实现步骤）
- [ ] 获得用户对计划的批准

询问：“公共接口应该长什么样？哪些行为最重要、最需要测试？”

**你无法测试一切。**与用户确认究竟哪些行为最重要。把测试精力集中在关键路径和复杂逻辑上，而非每一个可能的边界情况。

### 2. Tracer Bullet

写一个测试，确认系统的一件事：

```
RED:   Write test for first behavior → test fails
GREEN: Write minimal code to pass → test passes
```

这就是你的 tracer bullet——证明这条路径能端到端走通。

### 3. 增量循环

对每个剩余的行为：

```
RED:   Write next test → fails
GREEN: Minimal code to pass → passes
```

规则：

- 一次一个测试
- 只写刚好够通过当前测试的代码
- 不要预判未来的测试
- 让测试聚焦于可观察的行为

### 4. 重构

所有测试通过后，寻找 [重构候选项](refactoring.md)：

- [ ] 提取重复
- [ ] 深化模块（把复杂度藏到简单接口背后）
- [ ] 在自然的地方应用 SOLID 原则
- [ ] 思考新代码揭示了现有代码的什么问题
- [ ] 每完成一步重构就运行测试

**绝不在 RED 状态下重构。**先回到 GREEN。

## 每轮循环的检查清单

```
[ ] Test describes behavior, not implementation
[ ] Test uses public interface only
[ ] Test would survive internal refactor
[ ] Code is minimal for this test
[ ] No speculative features added
```
