---
name: writing-shape
description: 拿一份装着原始素材的 markdown 文件，通过一场对话式会话把它塑造成一篇文章——草拟候选开头，逐段把文章养大，在每一步都就格式（列表、表格、callout、引用）展开论证。当用户手上有一堆笔记、碎片或粗略草稿，并想要帮忙把它变成可发表的东西时使用。
---

<what-to-do>

用户已经传入（或将要传入）一份装着原始素材的 markdown 文件。把它当作输入素材堆——可以是一份整洁的碎片列表、一墙无结构的散文，或一份记录稿。格式无所谓。在做别的任何事之前，先从头到尾读完它。

然后运行一场塑造会话，产出一份独立的文章文档。不要编辑原始素材文件——它对本技能是只读的。

如果用户没说文章保存到哪里，问一次并记住路径。用户会在会话期间编辑文章文件；每次写入之前都要重新读取，以便保留他们的编辑。

</what-to-do>

<supporting-info>

## 循环

1. **读素材堆。** 完整读完输入文件。对里面有什么形成一个感觉。
2. **草拟 2—3 个候选开头。** 每个开头都应当隐含文章的一个不同立论或角度。把它们全部展示出来。逼用户挑一个或拼一个混合版。所选的开头定义了文章其余部分必须做什么。
3. **逐段养大。** 开头落定后，问「given this opening, what does the reader need to hear next?」从素材堆里抽材料来回答。就下一拍该是一段、一个列表、一张表格、一个 callout、一段引用还是一个代码块展开论证。每个格式选择都应当是有意为之且经得起辩护的。
4. **边写边追加到文章文件。** 不要攒批。每商定一段或一块就立即写下，让用户能看到文章逐渐成形。
5. **循环第 3 步直到文章完成。** 由用户决定何时算完成。

## 对话的感觉

这是一场反过来的拷问式会话。在构思中，问题是「what are you actually noticing?」在这里则是「what is this article actually arguing, and in what order does the reader need to hear it?」要顶回去。不让软弱的过渡轻易过关。如果一段配不上它的位置，就砍掉。

要反复使用的具体招法：

- "What does this paragraph do for the reader that the previous one didn't?"
- "If I cut this, what breaks?"
- "Is this prose, or should it be a list? Why prose?"
- "This sentence is doing two jobs — split it or pick one."
- "The opening promised X. We've drifted to Y. Either re-thread it or change the opening."

## 从素材堆里抽取

把原始素材当作采石场，而不是脚本。抽出一个碎片，改造它以贴合周围的段落，然后放进去。一个碎片可以拆散到多个段落，可以与另一个合并，也可以转述。素材堆的职责是被开采；文章的职责是读起来像一个声音。

如果素材堆缺了文章所需的某样东西，就明确点出这个缺口："We need an example here and the pile doesn't have one — give me one now or we cut this section."

## 真正该有的格式论证

在选择如何呈现某一拍时，把这些取舍出声地和用户掂量，而不是默默决定：

- **散文 vs. 列表。** 散文承载论证，列表承载并列的项。如果各项并非真正并列，散文更好。如果是，列表扫读起来更快。
- **行内 vs. callout。** 提示、警告和旁白放进 callout（`> [!TIP]`、`> [!NOTE]`）——但仅当它们行内放置确实会带偏主线论证时才放。否则就留在行内。
- **表格 vs. 重复结构。** 如果同一个形态带着相同字段重复 3 次以上，用表格。否则用带加粗引导词的散文。
- **引用 vs. 转述。** 当原文的措辞本身就是重点时引用。当只有意思重要时转述。
- **代码块 vs. 行内代码。** 多行、可运行或用于示例的 → 代码块。单个 token 或标识符 → 行内。

## 写作节奏

每商定一块就追加到文章文件。每次写入之前都从磁盘重新读取文件——用户可能在两轮之间做过编辑。绝不盲目覆盖。如果用户想重写某一段，就地编辑那一段；其余不动。

## 范围之外

- 挖掘素材堆里没有的新碎片（素材堆就是输入——如果它不完整，点出缺口，要么让用户补上，要么砍掉该节）。
- 编辑原始素材文件。
- 发布、为某个特定平台排版，或添加用户没要求的 frontmatter。

</supporting-info>
