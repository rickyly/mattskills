# 工程

我日常做代码工作时使用的技能。

## 用户触发

只有你手动输入时才能触达（`disable-model-invocation: true`）。

- **[ask-matt](./ask-matt/SKILL.md)** —— 询问哪个技能或哪条流程适合你当前的情境。它是本仓库中用户触发技能的路由器。
- **[grill-with-docs](./grill-with-docs/SKILL.md)** —— 拷问式会话，同时构建项目的领域模型，磨炼术语并就地更新 `CONTEXT.md` 与 ADR。
- **[triage](./triage/SKILL.md)** —— 让 issue 流经一套由分诊角色组成的状态机。
- **[improve-codebase-architecture](./improve-codebase-architecture/SKILL.md)** —— 扫描代码库寻找加深机会，以可视化 HTML 报告呈现，再就你挑选的那一项展开拷问。
- **[setup-matt-pocock-skills](./setup-matt-pocock-skills/SKILL.md)** —— 为这套工程技能配置本仓库（issue 追踪器、分诊标签、领域文档布局）。每个仓库运行一次。
- **[to-issues](./to-issues/SKILL.md)** —— 用纵向切片把任何计划、规格或 PRD 拆成可被独立认领的 issue。
- **[to-prd](./to-prd/SKILL.md)** —— 把当前对话转成 PRD 并发布到 issue 追踪器。
- **[prototype](./prototype/SKILL.md)** —— 构建一个一次性原型 —— 用于状态/逻辑问题的可运行终端应用，或几个可切换的 UI 变体。

## 模型触发

模型或用户均可触达（触发措辞丰富，便于模型主动选用）。

- **[diagnosing-bugs](./diagnosing-bugs/SKILL.md)** —— 针对疑难 bug 和性能回退的严谨诊断循环：复现 → 最小化 → 提出假设 → 插桩 → 修复 → 回归测试。
- **[tdd](./tdd/SKILL.md)** —— 采用 red-green-refactor 循环的测试驱动开发。一次一个纵向切片地构建特性或修复 bug。
- **[domain-modeling](./domain-modeling/SKILL.md)** —— 主动构建并磨炼项目的领域模型 —— 挑战术语、用场景压力测试、就地更新 `CONTEXT.md` 与 ADR。
- **[codebase-design](./codebase-design/SKILL.md)** —— 设计深模块时共享的规约与词汇：小接口、干净的接缝、可通过接口测试。
