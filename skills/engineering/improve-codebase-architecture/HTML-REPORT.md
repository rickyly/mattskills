# HTML 报告格式

架构评审会渲染为一个独立完整的 HTML 文件，存放在操作系统的临时目录中。Tailwind 和 Mermaid 都从 CDN 引入。Mermaid 能可靠地处理图形类的示意图；手工搭建的 div 和内联 SVG 则负责更具编排性的视觉效果（质量图、剖面图）。两者混用——不要事事都依赖 Mermaid，否则会开始显得千篇一律。

## 脚手架

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Architecture review — {{repo name}}</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script type="module">
      import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs";
      mermaid.initialize({ startOnLoad: true, theme: "neutral", securityLevel: "loose" });
    </script>
    <style>
      /* small custom layer for things Tailwind doesn't cover cleanly:
         dashed seam lines, hand-drawn-feeling arrow heads, etc. */
      .seam { stroke-dasharray: 4 4; }
      .leak { stroke: #dc2626; }
      .deep { background: linear-gradient(135deg, #0f172a, #1e293b); }
    </style>
  </head>
  <body class="bg-stone-50 text-slate-900 font-sans">
    <main class="max-w-5xl mx-auto px-6 py-12 space-y-12">
      <header>...</header>
      <section id="candidates" class="space-y-10">...</section>
      <section id="top-recommendation">...</section>
    </main>
  </body>
</html>
```

## 页头

仓库名、日期，以及一个紧凑的图例：实心框 = 模块，虚线 = seam，红色箭头 = 泄漏，粗的深色框 = 深模块。不要引言段落——直接进入候选项。

## 候选项卡片

示意图承载主要分量。散文要稀疏、平实，直接使用术语表（来自 `/codebase-design` 技能）中的术语，不加铺陈。

每个候选项是一个 `<article>`：

- **Title**——简短，点明这次深化（例如「合并 Order 收单流水线」）。
- **Badge row**——推荐强度（`Strong` = 翠绿，`Worth exploring` = 琥珀，`Speculative` = 石板灰），外加一个标注依赖类别的标签（`in-process`、`local-substitutable`、`ports & adapters`、`mock`）。
- **Files**——等宽列表，`font-mono text-sm`。
- **Before / After diagram**——核心部分。两栏，并排。见下方的模式。
- **Problem**——一句话。哪里痛。
- **Solution**——一句话。改了什么。
- **Wins**——要点列表，每条不超过 6 个词。例如「Tests hit one interface」「Pricing logic stops leaking」「Delete 4 shallow wrappers」。
- **ADR callout**（如适用）——琥珀色调框里的一行字。

不要成段的解释。如果一个示意图需要一段文字才能看懂，那就重画这个示意图。

## 示意图模式

挑选与候选项相称的模式。混用它们。不要让每张示意图都长得一样——多样性本身就是要点之一。

### Mermaid 图（依赖 / 调用流的主力）

当要点是「X 调用 Y、Y 调用 Z，看看这一团乱」时，使用 Mermaid 的 `flowchart` 或 `graph`。把它包进一张 Tailwind 风格的卡片里，免得显得像凭空插进来的。用 classDef 设置样式，把泄漏的边染成红色、把深模块染成深色。序列图很适合表现「之前：6 次往返；之后：1 次」。

```html
<div class="rounded-lg border border-slate-200 bg-white p-4">
  <pre class="mermaid">
    flowchart LR
      A[OrderHandler] --> B[OrderValidator]
      B --> C[OrderRepo]
      C -.leak.-> D[PricingClient]
      classDef leak stroke:#dc2626,stroke-width:2px;
      class C,D leak
  </pre>
</div>
```

### 手工搭建的框与箭头（当 Mermaid 的布局跟你较劲时）

把模块画成带边框和标签的 `<div>`。箭头用内联 SVG 的 `<line>` 或 `<path>` 元素，绝对定位在一个相对定位的容器之上。当你想让「之后」的示意图呈现为一个边框粗厚的深模块、内部细节被灰化时，就用这种方式——Mermaid 渲染不出那种恰到好处的分量感。

### 剖面图（适合表现分层的浅）

堆叠水平的横条（`h-12 border-l-4`）来展示一次调用穿过的各个层。之前：6 个各自什么都不做的薄层。之后：1 个粗厚的横条，标注着合并后的职责。

### 质量图（适合表现「interface 和 implementation 一样宽」）

每个模块两个矩形——一个表示 interface 的表面积，一个表示 implementation。之前：interface 矩形几乎和 implementation 矩形一样高（shallow）。之后：interface 矩形很矮，implementation 矩形很高（deep）。

### 调用图坍缩

之前：一棵函数调用树，渲染为嵌套的框。之后：同一棵树坍缩成一个框，原本的调用如今变为内部调用，在框内淡化显示。

## 样式指南

- 偏编排风格，而非企业仪表盘风格。留白充裕。标题可选用衬线字体（`font-serif` 与 stone/slate 搭配效果不错）。
- 用色克制：一个强调色（翠绿或靛蓝），外加红色表示泄漏、琥珀色表示警告。
- 把示意图高度保持在约 320px，这样「之前/之后」能舒适地并排放置而无需滚动。
- 示意图内部的模块标签用 `text-xs uppercase tracking-wider`——它们应当读起来像示意图标注，而非 UI。
- 唯一的脚本是 Tailwind CDN 和 Mermaid ESM 导入。除此之外报告是静态的——没有应用代码，除了 Mermaid 自身的渲染之外没有任何交互。

## 首要推荐小节

一张更大的卡片。候选项名称、一句话说明理由、指向其卡片的锚点链接。仅此而已。

## 语气

平实的英文，简洁——但架构上的名词和动词直接取自 `/codebase-design` 技能。简洁不是跑题的借口。

**Use exactly:** module, interface, implementation, depth, deep, shallow, seam, adapter, leverage, locality.

**Never substitute:** component, service, unit (for module) · API, signature (for interface) · boundary (for seam) · layer, wrapper (for module, when you mean module).

**Phrasings that fit the style:**

- "Order intake module is shallow — interface nearly matches the implementation."
- "Pricing leaks across the seam."
- "Deepen: one interface, one place to test."
- "Two adapters justify the seam: HTTP in prod, in-memory in tests."

**Wins bullets** 用术语表中的术语点明收益：*"locality: bugs concentrate in one module"*、*"leverage: one interface, N call sites"*、*"interface shrinks; implementation absorbs the wrappers"*。不要写 *"easier to maintain"* 或 *"cleaner code"*——这些词不在术语表里，配不上它们的位置。

不要含糊其辞，不要清嗓子式的铺垫，不要写「值得注意的是……」。如果一个句子能写成要点，就写成要点。如果一个要点可以删掉，就删掉。如果某个术语不在 `/codebase-design` 术语表里，在生造新词之前先找一个表内的术语。
