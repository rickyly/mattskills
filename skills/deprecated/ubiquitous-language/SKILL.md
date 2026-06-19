---
name: ubiquitous-language
description: 从当前对话中提取一份 DDD 风格的通用语言术语表，标记歧义并提出规范术语。保存到 UBIQUITOUS_LANGUAGE.md。当用户想要定义领域术语、构建术语表、强化术语、创建通用语言，或提到 "domain model" 或 "DDD" 时使用。
disable-model-invocation: true
---

# 通用语言

从当前对话中提取并形式化领域术语，整理成一份一致的术语表，保存到本地文件。

## 流程

1. **扫描对话**，寻找与领域相关的名词、动词和概念
2. **识别问题**：
   - 同一个词用于不同概念（歧义）
   - 不同的词用于同一个概念（同义词）
   - 含糊或被赋予过多含义的术语
3. **提出一份规范术语表**，给出有主张的术语选择
4. **写入工作目录中的 `UBIQUITOUS_LANGUAGE.md`**，使用下面的格式
5. **在对话中内联输出一份摘要**

## 输出格式

按以下结构编写 `UBIQUITOUS_LANGUAGE.md` 文件：

```md
# Ubiquitous Language

## Order lifecycle

| Term        | Definition                                              | Aliases to avoid      |
| ----------- | ------------------------------------------------------- | --------------------- |
| **Order**   | A customer's request to purchase one or more items      | Purchase, transaction |
| **Invoice** | A request for payment sent to a customer after delivery | Bill, payment request |

## People

| Term         | Definition                                  | Aliases to avoid       |
| ------------ | ------------------------------------------- | ---------------------- |
| **Customer** | A person or organization that places orders | Client, buyer, account |
| **User**     | An authentication identity in the system    | Login, account         |

## Relationships

- An **Invoice** belongs to exactly one **Customer**
- An **Order** produces one or more **Invoices**

## Example dialogue

> **Dev:** "When a **Customer** places an **Order**, do we create the **Invoice** immediately?"
> **Domain expert:** "No — an **Invoice** is only generated once a **Fulfillment** is confirmed. A single **Order** can produce multiple **Invoices** if items ship in separate **Shipments**."
> **Dev:** "So if a **Shipment** is cancelled before dispatch, no **Invoice** exists for it?"
> **Domain expert:** "Exactly. The **Invoice** lifecycle is tied to the **Fulfillment**, not the **Order**."

## Flagged ambiguities

- "account" was used to mean both **Customer** and **User** — these are distinct concepts: a **Customer** places orders, while a **User** is an authentication identity that may or may not represent a **Customer**.
```

## 规则

- **要有主张。** 当同一个概念存在多个词时，选出最佳的那个，并把其余的列为应避免的别名。
- **明确标记冲突。** 如果某个术语在对话中被歧义地使用，在 "Flagged ambiguities" 一节中点明，并给出清晰的建议。
- **只收录对领域专家有意义的术语。** 跳过模块名或类名，除非它们在领域语言中具有含义。
- **保持定义紧凑。** 最多一句话。定义它**是什么**，而不是它**做什么**。
- **展示关系。** 使用加粗的术语名，并在显而易见处表达基数。
- **只收录领域术语。** 跳过通用编程概念（array、function、endpoint），除非它们具有领域特定的含义。
- **当自然聚类出现时，把术语分成多张表**（例如按子域、生命周期或参与者）。每个分组各有自己的标题和表格。如果所有术语都属于单一内聚的领域，一张表也可以 —— 不要强行分组。
- **写一段示例对话。** 一段 dev 与领域专家之间的简短对话（3-5 轮），自然地演示这些术语如何相互配合。该对话应当厘清相关概念之间的边界，并展示术语被精确使用。

<example>

## Example dialogue

> **Dev:** "How do I test the **sync service** without Docker?"

> **Domain expert:** "Provide the **filesystem layer** instead of the **Docker layer**. It implements the same **Sandbox service** interface but uses a local directory as the **sandbox**."

> **Dev:** "So **sync-in** still creates a **bundle** and unpacks it?"

> **Domain expert:** "Exactly. The **sync service** doesn't know which layer it's talking to. It calls `exec` and `copyIn` — the **filesystem layer** just runs those as local shell commands."

</example>

## 重新运行

当在同一对话中再次触发时：

1. 读取现有的 `UBIQUITOUS_LANGUAGE.md`
2. 纳入后续讨论中出现的任何新术语
3. 如果理解已经演进，更新定义
4. 重新标记任何新出现的歧义
5. 重写示例对话以纳入新术语
