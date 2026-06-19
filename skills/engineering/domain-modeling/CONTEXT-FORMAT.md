# CONTEXT.md 格式

## 结构

```md
# {Context Name}

{One or two sentence description of what this context is and why it exists.}

## Language

**Order**:
{A one or two sentence description of the term}
_Avoid_: Purchase, transaction

**Invoice**:
A request for payment sent to a customer after delivery.
_Avoid_: Bill, payment request

**Customer**:
A person or organization that places orders.
_Avoid_: Client, buyer, account
```

## 规则

- **要有立场。**当同一个概念存在多个词时，选出最好的那个，把其余的列在 `_Avoid_` 下面。
- **定义要紧凑。**最多一到两句话。定义它「是」什么，而非它「做」什么。
- **只收录这个项目上下文特有的术语。**通用编程概念（超时、错误类型、工具模式）不该收录，即便项目大量用到它们。在添加一个术语前，先问：这是这个上下文独有的概念，还是一个通用编程概念？只有前者才该收录。
- **当出现自然的聚类时，把术语归到子标题下。**如果所有术语都属于同一个内聚领域，平铺成一个列表也无妨。

## 单上下文与多上下文仓库

**单上下文（多数仓库）：**仓库根目录下放一个 `CONTEXT.md`。

**多上下文：**仓库根目录下放一个 `CONTEXT-MAP.md`，列出各个上下文、它们所在的位置，以及它们之间的关系：

```md
# Context Map

## Contexts

- [Ordering](./src/ordering/CONTEXT.md) — receives and tracks customer orders
- [Billing](./src/billing/CONTEXT.md) — generates invoices and processes payments
- [Fulfillment](./src/fulfillment/CONTEXT.md) — manages warehouse picking and shipping

## Relationships

- **Ordering → Fulfillment**: Ordering emits `OrderPlaced` events; Fulfillment consumes them to start picking
- **Fulfillment → Billing**: Fulfillment emits `ShipmentDispatched` events; Billing consumes them to generate invoices
- **Ordering ↔ Billing**: Shared types for `CustomerId` and `Money`
```

技能会推断适用哪种结构：

- 如果 `CONTEXT-MAP.md` 存在，读取它以找到各个上下文
- 如果只存在一个根 `CONTEXT.md`，则为单上下文
- 如果两者都不存在，则在解析出第一个术语时按需创建一个根 `CONTEXT.md`

当存在多个上下文时，推断当前话题与哪一个相关。如果不清楚，就询问。
