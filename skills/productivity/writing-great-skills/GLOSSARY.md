# 术语表 —— 打造优秀的技能

关于什么造就一个优秀技能的领域模型。技能的存在是为了从随机系统中榨取出确定性；下面每个术语都是服务于这一目标的杠杆。这是 [`writing-great-skills`](SKILL.md) 的披露式参考。

任何定义中的**加粗术语**本身也是本术语表收录的词条；可通过它们的标题找到。

## 语言

### Predictability

技能让代理在每次运行时表现出相同行为**方式**的程度——是相同的过程，而非相同的输出（头脑风暴类技能就应当*可预测地*发散；它的 token 各不相同，但行为不变）。这是其他所有术语服务的根本美德——成本和可维护性是它的症状，而非与之竞争的对手。

*应避免*：consistency、reliability、robustness、output-determinism

### Model-Invoked

保留 **description** 字段的技能，因此代理能看到它并自主触发——而人类仍可输入它的名字，所以模型触发总是*包含*用户可达。不存在仅模型可达的状态：description 只会*增加*代理的可发现性，绝不会移除人类的可达。代价是每一轮都要为这份可发现性付出长期的**context load**。它可被其他技能触达，因为让它对代理可发现的那份 description 同样使它可被触发。一个内容全是 **reference** 的 model-invoked 技能也是共享 reference 的一处归宿：另一个技能可以触达它，因此多个技能都需要的 reference 可以集中存放在一处。只有当代理必须靠自己触达技能时才选择模型触发；如果它除了手动调用之外从不触发，就删掉 description，不付任何 context load。

*应避免*：ability、tool、capability

### User-Invoked

剥离了 **description** 的技能——对代理不可见，只能由人类输入它的名字触达（用户*独占*，而 **model-invoked** 是用户*和代理*共享）。用代理可发现性换取零 **context load**。因为它没有 description，除了人类之外什么都触达不到它：没有其他技能能触发它。

*应避免*：procedure、workflow、command

### Description

技能的机器可读触发器，也是一个 **model-invoked** 技能被迫始终保持加载的那一个 **context pointer**。它的存在本身*就是*那条触发轴线：保留它，技能就是 model-invoked（且可被其他技能触达）；删除它，技能就成为 **user-invoked**，只能由人类触达。它是 model-invoked 技能 **context load** 的来源。

*应避免*：frontmatter、summary

### Context Pointer

代理上下文中持有的一个引用，它指明某份位于上下文之外的材料，并编码了触达该材料的条件。**description** 是顶层的 context pointer（上下文窗口 → 技能）；指向披露文件的指针是同一对象在下一层的体现。决定代理*何时*触达——以及*有多可靠*——的是它的措辞，而非它指向的目标。一个必备的目标藏在措辞薄弱的指针后面，就是一个方差缺陷：先修措辞，只有在打磨措辞失败后才把材料内联进来。

*应避免*：link、reference、import

### Context Load

一个 **model-invoked** 技能强加给代理上下文窗口的成本——它的 **description** 始终被加载，既花费 token 也花费注意力。这正是 **user-invoked** 技能因没有 description 而得以逃脱的成本，也是拆分出更多 model-invoked 技能的刹车。

*应避免*：token cost、context bloat

### Cognitive Load

一个 **user-invoked** 技能强加给人类的成本——人类必须记在脑子里的东西：有哪些技能存在、何时该去用每一个（人类就是那个索引）。这正是 **model-invocation** 因对代理可发现而消除的成本，也是拆分出更多 user-invoked 技能的刹车。它不是一项应当最小化的成本：它是人类自主权的代价，是某些技能保持 user-invoked 的理由。在人类判断重要的地方花费它；在不重要的地方移除它。

*应避免*：human index、burden、overhead

### Granularity

你把技能划分得有多细。更细的划分会花费两种 load 之一：更多 **model-invoked** 技能花费 **context load**（更多 description 挤占窗口、争抢注意力）；更多 **user-invoked** 技能花费 **cognitive load**（人类要记住和触达的更多）。两种切法指导这种划分。按 **invocation** 切：当你有一个独特的 **leading word** 可以触发某个 model-invoked 技能时就把它拆出来——一个你在 prompt 里真正会用到的触发词。按 **sequence** 切：当一连串 **steps** 中某一步的 **post-completion steps** 需要被隐藏时就拆开它，因为把它隔离进自己的上下文能清空其后续内容。要警惕反向操作：合并序列会把每一步的 post-completion steps 暴露给后续内容，诱发过早完成。

*应避免*：chunking、modularity

### Router Skill

一个 **user-invoked** 技能，其职责是指向你的其他 user-invoked 技能——逐一指明每一个以及何时该去用它——这样人类只需记住一个技能，而不是许多。它只能提示，绝不能触发它们：user-invoked 技能没有 **description**，所以除了人类什么都触达不到它们。这是 user-invoked 技能数量倍增时治疗 **cognitive load** 的良方。

*应避免*：dispatcher、menu、registry、index、router procedure

### Information Hierarchy

技能内容按代理对它的需要有多即时来排序——一架单一的阶梯，由两次切割产生：在文件内还是藏在指针后，以及 step 还是 reference。各级阶梯：

- **Steps** —— 文件内，主要
- **Reference**，文件内 —— 次要
- **Reference**，已披露 —— 藏在 **context pointer** 后

一个没有 **steps** 的技能只用底部两级——往往是一组合理的扁平对等集合（例如一次评审的每条规则都在同一级），这是一种良好的安排，不是坏味道。这个层级与 invocation 无关：无论技能是全 steps、全 reference 还是两者兼有，它都可以是 model-invoked 或 user-invoked。当一个技能有 steps 时，本应被披露却留在文件内的 reference 会把它们埋没，让关注它们变成抛硬币的概率——这是一根方差杠杆，不只是可读性杠杆。让阶梯顶部保持清晰可读；能往下推的尽量往下推。

*应避免*：structure、organization、layout

### Co-location

把代理同时需要的材料放在一处——一个概念的定义、规则和注意事项归在同一个标题下，而非散落于整个文件——这样读到一部分就会把它的相邻内容一并带出。它是 **Information Hierarchy** 在文件内的伴侣：层级排定一段内容*往下落到多深*；co-location 决定它落定之后*旁边坐着什么*。一段 **reference** 的正确格式没有公式可循；检验标准是技能应当读起来像是为代理写的文档，而归拢好的材料读起来正是如此，散落的材料则不然。它有别于 **Duplication**：后者是把一个含义重复在两处，而散落是把单个含义碎裂分散到许多处。

*应避免*：grouping、clustering、cohesion

### Branch

技能可被触发的一种独特方式——技能处理的一种情形——因此不同的运行会走过它的不同路径。一个有许多 step 的技能可能携带许多 branch；一个线性的技能则一个都没有。

*应避免*：path、case、fork

### Progressive Disclosure

把 **reference** 沿阶梯往下移——移出 SKILL.md、藏到 **context pointer** 后面——以保持顶部清晰可读。它主要不是一种 token 优化；它是保护 **information hierarchy** 的手段。由 **branching** 授权：披露只有某些 branch 需要的内容，内联每条路径都需要的内容；如果一个指针在必备材料上触发不可靠，就打磨它的措辞，只有在那也失败时才把它拉回内联。

*应避免*：lazy loading、chunking

### Steps

代理执行的有序动作——当一个技能拥有它们时，它们是技能内容的主要层级，是赢得 SKILL.md 一席之地的部分。并非每个技能都有 steps：一个技能可以全是 steps（`tdd`）、全是 **reference**（一次评审）或两者兼有，与 invocation 无关。每一步都终结于一条 **completion criterion**，无论清晰还是含糊。

*应避免*：workflow、instructions、choreography

### Completion Criterion

告诉代理一个工作单元已完成的条件——它据以判断的目标。两项属性使它成为一根杠杆，而不只是一种质量。它的**清晰度**（代理能否分辨完成与未完成？）抵抗 **premature completion**——一个含糊的界限（"达成理解"）会让代理宣布完成并滑向下一步；这条轴线需要*步骤*才会起作用，因为过早完成是一种步骤之间的失败。它的**要求量**（它要求多少）设定 **legwork**——"每个被修改的 model 都被交代清楚"逼出彻底的工作，而"产出一份变更清单"则不会——而这条轴线*不*受步骤约束：它也能约束一段扁平的 reference，这正是一个没有 steps 的技能仍能携带穷尽标准（"每条规则都已应用"）的原因。最强的标准既可核查又能穷尽。

*应避免*：done condition、exit condition、stopping rule

### Post-Completion Steps

紧跟在当前步骤之后的那些 **steps**。当它们可见时，会把代理拉向 **premature completion**——代理看到的越多，拉力越强；防御办法是把步骤序列拆成两段，从而隐藏它们。

*应避免*：horizon、fog of war、lookahead

### Legwork

代理在单个步骤之内于幕后所做的工作——读文件、探索代码库、做改动、自己挖出所需的东西，而不是甩给用户。它存在于步骤结构之下：从不写成它自己的一步，潜伏在措辞之中，由代理而非技能控制。它是 **post-completion steps** 跨步骤拉力在步骤之内的对应物。由一个 **leading word**（*comprehensive*、*thorough*）或一条要求工作穷尽的 **completion criterion** 抬高——包括应用于扁平 reference 的要求量轴线，这正是驱动一个全是扁平 reference 的技能覆盖它所有级别的动力。当那份要求缺席时，或当 **premature completion** 把步骤截短时，它就会变薄。

*应避免*：scope、effort、diligence、coverage

### Reference

代理按需查阅的材料——定义、事实、参数、示例、条件性指令。当一个技能有 **steps** 时它从属于步骤；当一个技能没有 steps 时它就是全部内容；或者它完全存在于任何技能之外——见 **External Reference**。通过 **context pointers** 触达，是 **progressive disclosure** 的首要候选。

*应避免*：supporting material、docs、background

### External Reference

存在于技能系统之外的 **Reference**——一个普通文件，没有 **description**、没有 **steps**、不可被触发——任何技能都可以指向它。它是无需自行触发的共享 reference 的归宿，也是两个 **user-invoked** 技能唯一能共用的共享归宿，因为它们都没有 description，因此谁也触发不了谁。

*应避免*：doc、resource、knowledge base

### Leading Word

一个紧凑的概念——也叫 *Leitwort*——它已经活在模型的预训练里，代理在运行技能时会用它来思考。它通过调用模型已经持有的先验，以尽可能少的 token 编码一条行为原则（例如 *lesson*、*proximal zone of development*、*fog of war*、*tracer bullets*）。作为一个 token 反复出现、而非作为一个句子，它在整个技能中累积出一份分布式的定义，并锚定一整片行为区域。自创一个词在你清晰定义它时也行得通，但一个生造的词调动不了任何先验——你为定义付出的 token，换的正是一个预训练词免费给你的东西。优先去找一个已有的词。

一个 leading word 两次服务于 **predictability**。在正文中它锚定 **execution**——每次这个概念出现，代理都会去取相同的行为；而在扁平 reference 内部，它把注意力聚焦到某一类需要留意的东西上，每次运行都调用出正确的检查。在 **description** 中它锚定 **invocation**——而且不只在技能内部：当同一个词活在你的 prompt、你的文档和你的代码库里时，代理会把这份共享语言关联到技能上，并更可靠地触发它。用你真正想要这个技能时会用到的 leading word 来给 description 措辞。

*应避免*：keyword、term、motif

### Single Source of Truth

理想状态：每个含义都恰好存在于一处权威之地，因此对技能行为的一次更改就是一处的更改。**Duplication** 是它的违背。

*应避免*：home、canonical location

### Relevance

一行内容是否仍然关乎技能所做之事——决定保留什么的透镜。一行会因两种情形之一失去相关性：从不关乎任务（纯粹的铺陈，或一个本应被披露的 **branch**），或者变陈旧（随着它所描述的行为或世界变化而过时）。更短的技能更容易保持相关，因为每一行都更便宜去核查。它有别于 **no-op**：相关性问的是一行是否关乎任务，而非它是否改变行为。

*应避免*：load-bearing、staleness、freshness

## 失效模式

### Premature Completion

在当前步骤真正完成之前就结束它，因为代理的注意力从工作滑向了完成本身。这是一种步骤之间的失败：它需要 **steps** 才会发生——一个没有步骤却提前收手的技能不是过早完成，而是在未被满足的要求下变薄的 **legwork**。这是两股力量之间的拔河：可见的 **post-completion steps**（向前的拉力）和 **completion criterion** 的清晰度（阻力——一道锐利可核查的界限能稳住，含糊的界限会让步）。模糊是必要条件：一道锐利的界限无论后续步骤可见多少都能抵抗拉力，所以一个从不抢跑的步骤无需防御。两根杠杆能稳住一个会抢跑的步骤，但要按顺序去取它们：**先打磨界限**——它是局部且廉价的。只有当标准无可救药地模糊*且*你确实观察到抢跑时，你才去**隐藏后续步骤**——而隐藏只有跨越一道真正的上下文边界才奏效（一次 user-invoked 的交接或一次子代理派发；一次内联的 model-invoked 调用会把后续步骤留在上下文里，什么也清不掉）。它是 legwork 变薄的一种成因，但有别于它：即便一个步骤跑到完全完成，legwork 也可能很薄。

*应避免*：premature closure、the rush、rushing、shortcutting

### Duplication

同一个含义被赋予了不止一处 **single source of truth**。它花费维护（改一处，你必须改其他处）、花费 token，并夸大显著性——重复一个含义会把它在阶梯上的权重抬高到超过其真实排名。它是 **leading word** 的偶然反面，后者通过重复一个 token、而非含义，刻意提升注意力。

*应避免*：repetition、redundancy

### Sediment

沉积在技能中、从未被清理的旧内容层，因为添加感觉安全而移除感觉有风险——于是陈旧无关的行不断堆积，你必须层层挖下去才能找到仍然鲜活的东西。这是任何没有修剪纪律的技能的默认命运；它是 **relevance** 的缓慢侵蚀，与 **duplication** 的重复含义相对。

*应避免*：accretion、bloat、cruft、rot

### Sprawl

一个单纯太长的技能——SKILL.md 里的行数太多——与它们是否陈旧或重复无关。即便一个全鲜活、全独特的技能也会冗长蔓延。它花费可读性（代理要趟过更多内容才能行动，注意力被多余内容稀释）、可维护性（每一行多出来的行都是又一行要保持 **relevant**），还浪费 token。良方是 **information hierarchy**：把 **reference** 推到 **context pointers** 后面，并按 **branch** 或序列拆分，使每条路径只携带它需要的东西。它有别于 **sediment**（长度来自陈旧堆积）和 **duplication**（长度来自重复含义）——sprawl 就是长度本身，不论其成因。

*应避免*：bloat、length、size、verbosity

### No-Op

一条什么都不改变的指令，因为模型默认就已经那样做了——你付出 load 去告诉代理它本来就会做的事。检验标准：相对默认行为，这一行是否改变行为？一行可以完全 **relevant** 却仍是 no-op。让 **leading word** 免费的那些先验，同样让一个 no-op 变得毫无价值。

leading word 是一种*技术*；No-Op 是对一行的一项*裁决*——两者会交叉。一个弱到打不过默认的 leading word 就是一个 no-op（在代理本就大致够彻底时还说 *be thorough*），修法是换一个能通过裁决的更强的词（*relentless*），而非换一种技术。所以 No-Op 检验——相对默认行为它是否改变行为？——也是你评判一个 leading word 是否对得起它那些重复的方式。这是相对模型而言、而非相对读者而言的：两个人在一行是否为 no-op 上意见相左，其实是在默认行为上意见相左，靠运行技能来定夺，而非靠辩论。

*应避免*：redundant instruction、restating the obvious、belaboring
