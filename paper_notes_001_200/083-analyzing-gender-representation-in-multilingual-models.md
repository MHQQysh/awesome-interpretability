# 083. Analyzing Gender Representation in Multilingual Models

> 人工精读笔记：以 mBERT 的英语、法语、西班牙语表示为对象，重点区分“性别可预测”与“性别可移除”这两个并不等价的问题。

## 0. 论文信息

- **作者**：Hila Gonen, Shauli Ravfogel, Yoav Goldberg
- **来源**：[RepL4NLP 2022](https://aclanthology.org/2022.repl4nlp-1.8)
- **模型/数据**：mBERT；BiosBias 与 Multilingual BiosBias（英语、法语、西班牙语）
- **工具**：线性 probe、Iterative Nullspace Projection（INLP）、PCA、方向余弦相似度

## 1. Introduction：要解决什么问题

多语言模型能把不同语言映射到共享表示空间，但“共享”到底意味着什么并不清楚。论文以性别这一可解释概念为案例，研究三件事：一个语言训练的 gender classifier 能否跨语言迁移？在一个语言中用 INLP 去除性别后，去除效果能否跨语言迁移？不同语言的 gender subspace 是否由相同方向组成？

这个问题同时关联可解释性与 debiasing：如果探针跨语言效果很好，人们可能推断性别信息完全共享；但如果用一个语言的投影不能在另一个语言中去除 gender，说明“可预测”与“可消除”需要看表示空间中全部方向，而不能只看一个 probe 分数。

## 2. Method / Framework：怎么解决

### 2.1 跨语言线性 probe

从 mBERT 最终层上下文表示训练 logistic regression gender classifier。训练语言和测试语言分离，考察 English->French、English->Spanish 以及反方向的 zero-shot transfer。高跨语言准确率意味着至少存在共享、线性可访问的 gender direction。

### 2.2 INLP 去除

INLP 迭代训练线性分类器，收集其权重方向并投影到 nullspace，使表示不能再被线性预测为 gender。作者分别在英语、法语、西班牙语训练 INLP，再把所得 neutralization projection 应用于其他语言；同时监测 profession classification，区分去除性别与破坏全部信息。

### 2.3 更细的子空间分析

作者把 gender 表示视为一组按预测能力排序的方向，用 PCA/解释方差观察一个语言的 gender rowspace 与另一个语言的 gender-neutral nullspace 的重叠；再比较 INLP 前 100 个 classifier 方向的两两 cosine similarity。这样可以解释为什么前几个方向共享而后续方向语言特定。

## 3. Baseline / 对比基线

- **Majority baseline**：INLP 后的 gender accuracy 是否降到多数类水平。
- **In-language INLP**：在训练 INLP 的同一语言上去除，是可达到的 debiasing 参照。
- **Cross-language INLP**：把另一语言训练的投影迁移过去，检验跨语言可移除性。
- **Random projection**：与相同维度的随机方向比较，排除解释方差下降只是维度减少造成。
- **Profession classifier**：gender 与职业在 BiosBias 中相关，职业准确率用于观察投影是否造成不必要的信息损伤。

## 4. Experiments / Findings：结果如何读

### 4.1 Gender probe transfer 很强

表 2 显示 gender classifier 跨语言迁移效果很高。例如 English classifier 在 French/Spanish 数据上的准确率约为 98.10%/97.29%，反方向也保持高水平。这个结果说明 mBERT 中存在跨语言共享的、线性可访问的 gender 信息。

但它只回答“能不能预测”，没有回答“所有 gender 方向是否相同”。这是论文后续反直觉结果的关键。

### 4.2 Gender removal transfer 很弱

表 3 中，在各语言内训练的 INLP 可以把 gender accuracy 从很高水平压到接近 majority；例如英语由约 99.3 降到约 53.7，profession accuracy 只从约 79.9 降到 78.1，说明 in-language 去除相对选择性。

把英语训练的 INLP 应用于法语/西班牙语时，gender 仍保持较高可预测性；法语/西班牙语训练的投影在英语上也不能完全消除 gender。表 4 显示 profession classification 的跨语言变化通常较小，说明失败并非单纯因为投影摧毁了整个表示。

### 4.3 性别子空间是 partial sharing

解释方差实验中，把 English gender rowspace 再投影到 French/Spanish neutral space 会丢掉一部分、但不是全部信息；随机投影控制排除了纯维度效应。方向相似度更细地显示，English/French 前三个 INLP classifier 的 cosine similarity 约为 0.777、0.597、0.453，而所有前 100 个方向的平均绝对相似度仅约 0.037。

因此最显著的前 2--3 个 gender directions 跨语言共享，足以让线性 probe transfer；较弱的后续方向语言特定，仍足以在移除共享方向后恢复 gender。论文把这一结构称为 partial sharing。

## 5. Ablation / 机制解释

random projection 是主要 sanity check；在相同维度下，跨语言 neutralization 比随机投影额外丢失信息，支持“确实存在共享 gender directions”的解释。EnGender + EnNeutral 等自投影应把 gender explained variance 归零，实验符合这一预期；而 EnGender + FrNeutral 不归零，证明跨语言 neutralization 不是完全对齐。

这也解释了“分类可迁移、去除不可迁移”的表面矛盾：预测只需一个强共享方向；去除则必须覆盖整个 gender 子空间，任何语言特定残余方向都可能被新的线性 probe 利用。

## 6. Limitations / 局限与复现注意

- 研究只覆盖 mBERT 和三种语言，不能直接推广到所有多语言模型、语言家族或 gender marking 体系。
- INLP 针对线性可解码信息；非线性 probe 仍可能恢复被称为“neutral”的信息。
- BiosBias 的职业与性别存在数据相关性，profession accuracy 不是完整的公平性评估。
- 论文测量的是表示空间中的可预测/可移除性，不直接证明模型在生成或真实决策中会使用同一方向。

## 7. 两句话总结

论文发现 mBERT 中的性别信息在英语、法语和西班牙语之间具有很强的线性可迁移性，但用一个语言训练的 INLP 却不能在另一个语言中完整去除它。细粒度方向分析解释了这一现象：最显著的 gender directions 共享，剩余较弱方向语言特定，因此“识别”只需部分共享而“中和”需要整体共享。
