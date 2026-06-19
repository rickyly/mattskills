---
name: writing-fragments
description: 一场拷问式会话，从用户身上挖掘碎片（fragment）——形态各异的写作素材小块（论断、小品、犀利的句子、半成形的想法）——并把它们追加到一份文档中，作为未来文章的原始素材。当用户想在强加结构之前先发展想法，或提到「fragments」「ideate」「raw material」等写作相关说法时使用。
---

<what-to-do>

运行一场产出碎片的拷问式会话。围绕用户想写的任何东西，不留情面地访问他们。不要强加阶段、提纲或结构——那明确不在范围之内。

当碎片从对话双方任一侧浮现时，把它们追加到一个 markdown 文件中。用户会在会话期间编辑这个文件；每次写入之前都要重新读取，以便保留他们的编辑。

如果用户没有传入路径，问一次文档保存到哪里，然后在本次会话余下时间里记住它。

从用户说的第一句话起就开始捕捉碎片，包括最初的提示词。

首次写入时，在顶部放一个 H1，写一个临时标题（之后可以改），其余什么都不放——没有元数据、没有目录、没有日期。

</what-to-do>

<supporting-info>

## 什么是碎片

一个碎片是任何可能存活进最终文章的文本片段。它必须_对作者可读_——作者能看懂它的意思——但它无需定义自己的术语，也无需让一个毫无背景的读者也能看懂。门槛是「这是不是一段好的写作？」，而不是「这是不是一个自成一体的论证？」

碎片是刻意保持形态各异的。可以成为碎片的例子：

- 一句你想用在某处、却还不知道用在哪里的犀利句子。
- 一个带一行论据的论断。
- 一个小品：一件发生过的事、一段代码片段、一个场景、一个类比。
- 一个半成形的想法："something about how X feels like Y, work this out later."
- 一句引文、一段对白、一句无意间听到的话。
- 一组凭感觉凑在一起的相关观察。
- 一句抱怨、一句坦白、一句妙语。

小说家的日记就是这个范本：多年来无结构的随手记，日后被挖掘为原始素材。碎片就是这些随手记。

## 文件格式

```markdown
# Working title

A first fragment lives here.

It can be multiple paragraphs. It can include lists, code, quotes — whatever
shape the fragment naturally takes.

---

A second fragment.

---

> A quoted line that the user wants to keep around.

A reaction to it.

---

- A cluster of related observations
- That hang together by feel
- And want to be near each other
```

碎片之间用水平分隔线（`\n---\n`）分隔。正文内不用标题。不用标签。除了添加顺序之外没有别的排序。

## 写作节奏

静默追加。不要为每个碎片征求许可。顺带提一下你加了什么（"adding that"），但不要用保存对话框打断交谈。

每次写入之前：从磁盘重新读取文件。用户可能在两轮之间编辑、重排或删除了碎片——保留他们的改动。绝不覆盖文件；只追加（或者，如果用户要求，就地编辑某个特定碎片）。

用户随时可以说「cut the last one」「rewrite that one sharper」「merge those two」。把这些当作一等指令对待。

</supporting-info>
