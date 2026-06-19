---
name: qa
description: 交互式 QA 会话，用户以对话方式报告 bug 或问题，代理据此创建 GitHub issue。会在后台探索代码库以获取上下文和领域语言。当用户想要报告 bug、做 QA、以对话方式创建 issue，或提到 "QA session" 时使用。
---

# QA 会话

进行一场交互式 QA 会话。用户描述他们遇到的问题。你负责澄清、探索代码库以获取上下文，并创建持久、以用户为中心、使用项目领域语言的 GitHub issue。

## 对用户提出的每个 issue

### 1. 倾听并适度澄清

让用户用自己的话描述问题。**最多提 2-3 个简短的澄清问题**，聚焦于：

- 他们的预期 vs 实际发生了什么
- 复现步骤（如果不明显）
- 问题是稳定复现还是间歇出现

不要过度访谈。如果描述已经清楚到足以创建 issue，就继续往下走。

### 2. 在后台探索代码库

在与用户交谈的同时，在后台启动一个 Agent（subagent_type=Explore）来理解相关区域。目标不是找到修复方案 —— 而是：

- 学习该区域使用的领域语言（查看 UBIQUITOUS_LANGUAGE.md）
- 理解该功能应当做什么
- 识别面向用户的行为边界

这份上下文能帮你写出更好的 issue —— 但 issue 本身不应引用具体文件、行号或内部实现细节。

### 3. 评估范围：单一 issue 还是拆分？

创建之前，先判断这是一个**单一 issue**，还是需要**拆分**成多个 issue。

在以下情况拆分：

- 修复跨越多个相互独立的区域（例如"表单校验有误，而且成功提示缺失，而且跳转坏了"）
- 存在明显可分离、不同人可以并行处理的关注点
- 用户描述的事物有多个不同的失败模式或症状

在以下情况保持为单一 issue：

- 这是一处地方的一种行为出了问题
- 这些症状全都源于同一个根本行为

### 4. 创建 GitHub issue

用 `gh issue create` 创建 issue。不要先让用户审阅 —— 直接创建并分享 URL。

issue 必须**持久** —— 即使经历大规模重构后它们仍应说得通。从用户的视角来写。

#### 单一 issue

使用以下模板：

```
## What happened

[Describe the actual behavior the user experienced, in plain language]

## What I expected

[Describe the expected behavior]

## Steps to reproduce

1. [Concrete, numbered steps a developer can follow]
2. [Use domain terms from the codebase, not internal module names]
3. [Include relevant inputs, flags, or configuration]

## Additional context

[Any extra observations from the user or from codebase exploration that help frame the issue — e.g. "this only happens when using the Docker layer, not the filesystem layer" — use domain language but don't cite files]
```

#### 拆分（多个 issue）

按依赖顺序创建 issue（阻塞项在先），这样你才能引用真实的 issue 编号。

每个子 issue 使用以下模板：

```
## Parent issue

#<parent-issue-number> (if you created a tracking issue) or "Reported during QA session"

## What's wrong

[Describe this specific behavior problem — just this slice, not the whole report]

## What I expected

[Expected behavior for this specific slice]

## Steps to reproduce

1. [Steps specific to THIS issue]

## Blocked by

- #<issue-number> (if this issue can't be fixed until another is resolved)

Or "None — can start immediately" if no blockers.

## Additional context

[Any extra observations relevant to this slice]
```

拆分时：

- **优先用许多薄 issue，而非少数厚 issue** —— 每个都应可独立修复和验证
- **诚实标注阻塞关系** —— 如果 issue B 在 issue A 修复前确实无法测试，就说明。如果它们相互独立，把两者都标为 "None — can start immediately"
- **按依赖顺序创建 issue**，这样你才能在 "Blocked by" 中引用真实的 issue 编号
- **最大化并行度** —— 目标是让多个人（或多个代理）能同时领取不同的 issue

#### 所有 issue 正文的通用规则

- **不写文件路径或行号** —— 它们会过时
- **使用项目的领域语言**（如果存在，查看 UBIQUITOUS_LANGUAGE.md）
- **描述行为，而非代码** —— 写"同步服务未能应用该补丁"，而非 "applyPatch() throws on line 42"
- **复现步骤是必需的** —— 如果你无法确定，就询问用户
- **保持简洁** —— 开发者应能在 30 秒内读完该 issue

创建完成后，打印所有 issue URL（并汇总阻塞关系），然后询问："下一个 issue，还是到此为止？"

### 5. 继续会话

持续进行，直到用户表示完成。每个 issue 都是独立的 —— 不要把它们批量处理。
