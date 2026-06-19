---
name: request-refactor-plan
description: 通过用户访谈创建一份包含微小提交的详细重构计划，然后将其作为 GitHub issue 创建。当用户想要规划一次重构、创建一份重构 RFC，或把一次重构拆分成安全的增量步骤时使用。
---

当用户想要创建一个重构请求时，会触发本技能。你应当按照下面的步骤进行。如果你认为某些步骤没有必要，可以跳过。

1. 请用户给出对他们想解决的问题的一段详尽描述，以及任何可能的解决思路。

2. 探索仓库，以核实他们的论断并理解代码库的现状。

3. 询问他们是否考虑过其他方案，并向他们提出其他方案。

4. 就实现方案访谈用户。要极其详尽和周全。

5. 敲定实现的确切范围。理清你计划改动什么、计划不改动什么。

6. 在代码库中查看该区域是否有测试覆盖。如果测试覆盖不足，询问用户的测试计划是什么。

7. 把实现拆分成一个由微小提交构成的计划。记住 Martin Fowler 的建议："make each refactoring step as small as possible, so that you can always see the program working."

8. 用重构计划创建一个 GitHub issue。issue 描述使用以下模板：

<refactor-plan-template>

## Problem Statement

The problem that the developer is facing, from the developer's perspective.

## Solution

The solution to the problem, from the developer's perspective.

## Commits

A LONG, detailed implementation plan. Write the plan in plain English, breaking down the implementation into the tiniest commits possible. Each commit should leave the codebase in a working state.

## Decision Document

A list of implementation decisions that were made. This can include:

- The modules that will be built/modified
- The interfaces of those modules that will be modified
- Technical clarifications from the developer
- Architectural decisions
- Schema changes
- API contracts
- Specific interactions

Do NOT include specific file paths or code snippets. They may end up being outdated very quickly.

## Testing Decisions

A list of testing decisions that were made. Include:

- A description of what makes a good test (only test external behavior, not implementation details)
- Which modules will be tested
- Prior art for the tests (i.e. similar types of tests in the codebase)

## Out of Scope

A description of the things that are out of scope for this refactor.

## Further Notes (optional)

Any further notes about the refactor.

</refactor-plan-template>
