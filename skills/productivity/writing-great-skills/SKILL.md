---
name: writing-great-skills
description: 编写和编辑技能的参考——让技能可预测的词汇与原则。
disable-model-invocation: true
---

技能的存在是为了从随机系统中榨取出确定性。**Predictability**——代理每次运行都走相同的*过程*，而非产出相同的输出——是根本美德；下面每根杠杆都服务于它。

**加粗术语**在 [`GLOSSARY.md`](GLOSSARY.md) 中有定义；到那里查阅完整含义。

## 触发

两种选择，各自权衡不同的成本：

- 一个 **model-invoked** 技能保留一份 **description**，因此代理能自主触发它，*而且*其他技能也能触达它（你仍然也可以输入它的名字）。它贡献于 **context load**——这份 description 每一轮都坐在窗口里。机制：省略 `disable-model-invocation`，并写一份面向模型的 description，配上丰富的触发措辞（"Use when the user wants…, mentions…"）。
- 一个 **user-invoked** 技能从代理的可达范围里剥离了 description：只有你输入它的名字才能触发它——其他技能都不能。零 context load，但它花费 **cognitive load**：*你*就是那个必须记住它存在的索引。机制：设置 `disable-model-invocation: true`；`description` 变为面向人类——一行摘要，触发列表被剥离。

只有当代理必须靠自己触达技能、或另一个技能必须触达它时，才选择模型触发。如果它只会被手动触发，就让它成为 user-invoked，不付任何 context load。

当 user-invoked 技能多到超出你能记住的范围时，那堆积起来的 cognitive load 由一个 **router skill** 治疗：一个 user-invoked 技能，逐一指明其他技能以及何时该去用每一个。

## 编写 description

一份 model-invoked 的 **description** 做两件事——陈述这个技能是什么，并列出应当触发它的那些 **branches**。每一个词都增加 **context load**，所以 description 比正文值得更狠的修剪：

- **把技能的 leading word 前置**——description 正是它做 invocation 工作的地方。
- **每个 branch 一个触发器。** 把单个 branch 改头换面的同义词是 **duplication**——"build features using TDD … asks for test-first development" 就是同一个 branch 写了两遍。把它们合并；只保留真正不同的 branch。
- **砍掉正文里已有的身份描述。** description 只保留触发器，外加任何"当另一个技能需要……"的可达从句。

## Information hierarchy

一个技能由两种内容类型构成——**steps** 和 **reference**——它们自由混合：一个技能可以全是 steps、全是 reference 或两者兼有。核心决策是用哪一种、以及每一种坐在 **information hierarchy** 的何处，这是一架按代理对材料的需要有多即时来排序的阶梯：

1. **In-skill step** —— `SKILL.md` 中的一个有序动作，主要层级：代理依序所做之事。每一步都终结于一条 **completion criterion**，即告诉代理工作已完成的条件。让它*可核查*（代理能否分辨完成与未完成？），并在重要之处让它*穷尽*（"每个被修改的 model 都被交代清楚"，而非"产出一份变更清单"）——一条含糊的标准会招来 **premature completion**。
2. **In-skill reference** —— `SKILL.md` 中按需查阅的一条定义、规则或事实。往往是一组合理的扁平对等集合（一次评审的每条规则都在同一级）——一种良好的安排，不是坏味道。*这个技能全是 reference。*
3. **External reference** —— 被推出 `SKILL.md`、移入一个单独文件的 reference，通过一个 **context pointer** 触达，只在指针触发时加载。（涵盖*已披露*的 reference——一个像 `GLOSSARY.md` 这样的同级文件，仍然是技能的一部分——直到完全 **external reference**，后者存在于技能系统之外、任何技能都可指向它。）

一条要求苛刻的 completion criterion 会驱动彻底的 **legwork**——代理在工作之内所做的挖掘——无论技能是否有步骤，因为"每条规则都已应用"约束扁平 reference，正如"每一步都已完成"约束一个序列。

往下推得太少，顶部就臃肿；往下推得太多，你就藏起了代理实际需要的材料。这份张力就是整个决策。

**Progressive disclosure** 是沿阶梯往下的那一步——移出 `SKILL.md`、进入一个被链接的文件——以保持顶部清晰可读。机制：技能文件夹里一个被链接的 `.md` 文件，按它所装的内容命名（这个技能把它的完整定义披露到 `GLOSSARY.md`）。有些技能会以不止一种方式被使用，而每一种独特的方式都是一个 **branch**——不同的运行走过技能的不同路径。Branching 是最干净的披露检验：内联每个 branch 都需要的内容，把只有某些 branch 会触达的内容推到一个指针后面。一个 **context pointer** 的*措辞*，而非它的目标，决定代理何时、以及多可靠地触达那份材料。

阶梯决定一段内容*往下落到多深*，而 **co-location** 决定它落定之后*旁边坐着什么*：把一个概念的定义、规则和注意事项归在同一个标题下，而非散落各处，这样读到一部分就会把它的相邻内容一并带出。

## 何时拆分

**Granularity** 是你把技能划分得有多细，而每一次切割都花费两种 load 之一，所以只有当切割值回票价时才拆。两种切法：

- **按 invocation** —— 当你有一个独特的 **leading word** 应当靠它自己触发某个 **model-invoked** 技能时、或另一个技能必须触达它时，就把它拆出来。你要为新增的、始终加载的 **description** 付出 **context load**，所以那份独立的可达必须值得。
- **按 sequence** —— 当前方仍待执行的步骤（某一步的 **post-completion steps**）诱使代理抢跑它面前那一步（**premature completion**）时，就拆开这一连串 **steps**。把它们移出视野会鼓励代理在当前任务上做更多 **legwork**。

## 修剪

让每个含义保持在一处 **single source of truth**：一处权威之地，这样改变行为就是一处的编辑。

逐行检查 **relevance**：它是否仍然关乎技能所做之事？

然后逐句、而非仅逐行地猎杀 **no-ops**：对每个孤立的句子运行 no-op 检验，当一句不通过时，删掉整句而不是从中删词。要狠——大多数不通过的散文应当删掉，而非重写。

## Leading words

一个 **leading word** 是一个已经活在模型预训练里、代理在运行技能时会用它来思考的紧凑概念（例如 *lesson*、*fog of war*、*tracer bullets*）。在全文中反复出现（虽然不一定——一个强力的 leading word 也许只需出现一次），它累积出一份分布式的定义，并以尽可能少的 token 锚定一整片行为区域，靠的是调动模型已经持有的先验。

它两次服务于 predictability。在正文中它锚定*execution*：每次这个词出现，代理都会去取相同的行为。在 description 中它锚定*invocation*：当同一个词活在你的 prompt、文档和代码里时，代理会把这份共享语言关联到技能上，并更可靠地触发它。

去寻找把技能重构为使用 leading word 的机会。一个在三处都铺陈出来的三元组（**duplication**）、一份花一整句去暗示一个想法的 description——每一处都是一段恳求**坍缩**进单个 token 的段落。例子包括：

- "fast, deterministic, low-overhead" -> *tight*——一个贯穿某阶段反复重述的品质——坍缩进单个预训练词（一个 *tight* 循环）。
- "a loop you believe in" -> *red*——把一道模糊的关卡转化为一个二元可观测状态（循环遇到 bug 就变*红*，要么变要么不变）。

你赢两次：更少的 token，*而且*一个更锐利的、供代理挂起其思考的钩子。假设每个技能都携带着 leading word 可以退役的重述——去把它们找出来。

## 失效模式

用这些来诊断用户在使用技能时可能遇到的问题。

- **Premature completion** —— 在一步真正完成之前就结束它，注意力滑向了*完成本身*。防御，按顺序：先打磨 completion criterion（廉价、局部）；只有当它无可救药地模糊*且*你观察到抢跑时，才通过拆分隐藏 post-completion steps（序列切法）。
- **Duplication** —— 同一个含义出现在不止一处。它花费维护和 token，并把一个含义在阶梯上的显著性抬高到超过其真实排名。
- **Sediment** —— 沉积下来的陈旧层，因为添加感觉安全而移除感觉有风险。这是任何没有修剪纪律的技能的默认命运。
- **Sprawl** —— 一个单纯太长的技能，即便每一行都鲜活且独特。它伤害可读性和可维护性，并浪费 token。良方是那架阶梯：把 **reference** 披露到指针后面，并按 **branch** 或序列拆分，使每条路径只携带它需要的东西。
- **No-op** —— 一行模型默认就已遵守的内容，于是你付出 load 却什么也没说。检验标准：相对默认行为它是否改变行为？一个弱的 leading word（在代理本就大致够彻底时还说 *be thorough*）是一个 no-op；修法是换一个更强的词（*relentless*），而非换一种技术。
