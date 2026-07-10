# 大模型可解释性逐篇阅读笔记

> 说明：本文档按每 10 篇一个批次维护。当前为第 1 批（1-10）。本批逐篇阅读了论文 PDF 的摘要、引言、方法和实验/结论段落，并结合 ACL Anthology 的正式元数据整理。文中的“摘要”是中文压缩概括，不是对原文摘要的逐字复制。

## 第 1 批：从行为诊断到可干预特征（1-10）

### 本批体现的领域发展流程

1. **先发现可观察的异常行为**：研究从重复生成、选择位置偏置、幻觉等输出层问题出发，先建立可复现的现象和评价协议。
2. **再定位内部表征**：通过 probing、层间比较、logit lens 等方法，判断语言结构、情绪、重复和偏置分别在哪些层或模块中表现出来。
3. **从相关性走向因果性**：activation patching、向量编辑、注意力重校准和特征 steering 被用来验证“改动这个内部信号，行为是否真的改变”。
4. **从多义神经元走向稀疏特征**：SAE/k-SAE 把混杂的激活分解为更容易命名和定位的特征，为概念级干预和知识/隐私编辑提供更细的操作单位。
5. **进入安全与可信应用**：解释性不再只是可视化，而是被用于隐私泄漏抑制、RAG 归因偏差分析、幻觉基准构造和模型行为控制。

这十篇论文共同说明：当前主线已经从“模型里面有什么”推进到“哪些内部因素导致了行为，以及能否以较小代价修改它”。同时，probing 的高准确率并不自动等于线性可读或因果解释，后续仍需要干预实验、反事实评估和跨模型复现。

## 1. Evaluating Sparse Autoencoders for Monosemantic Representation

- **作者 / 发表**：Moghis Fereidouni, Muhammad Umair Haider, Peizhong Ju, A.B. Siddique；Findings of EACL 2026
- **论文链接**：[ACL Anthology](https://aclanthology.org/2026.findings-eacl.313/)
- **方向**：SAE、monosemanticity、概念级干预
- **要解决的问题**：SAE 常被认为可以缓解大模型神经元的 polysemanticity，但此前缺少把 SAE 与原始模型进行统一、定量比较的指标；同时，特征分解是否真的带来更精确的概念控制，也需要直接验证。
- **怎么解决**：论文提出基于 Jensen-Shannon 距离的 concept separability score，从激活分布角度比较原始模型和 SAE 特征。作者在 Gemma-2-2B、DeepSeek-R1、多种 SAE 和五类数据上评估，并比较完整 masking、部分 suppression 以及新提出的 APP（Attenuation via Posterior Probabilities）干预。
- **摘要概括**：实验显示 SAE 特征具有更高的概念可分性，能够降低多义性；在概念删除任务中，部分抑制比整神经元屏蔽更精确。APP 利用概念条件激活分布进行定向抑制，在有效删除概念的同时带来最小的困惑度上升。
- **阅读要点**：这篇工作把“SAE 特征更可解释”的定性判断推进为分布距离和干预效果两个可量化标准。它的价值不只在 feature discovery，也在于把解释单元直接连接到了模型编辑与控制。

## 2. Mechanistic Interpretability of Emotion Inference in Large Language Models

- **作者 / 发表**：Ala N. Tak, Amin Banayeeanzade, Anahita Bolourani, Mina Kian, Robin Jia, Jonathan Gratch；Findings of ACL 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.findings-acl.679/)
- **方向**：机制可解释性、情绪表征、activation patching、情感安全
- **要解决的问题**：LLM 能从文本推断人类情绪，但模型究竟通过哪些内部区域处理情绪刺激，以及这些表征是否具有心理学意义，并不清楚。
- **怎么解决**：作者在多个模型家族和规模上进行 emotion probing，检查情绪表征的层级位置，并用 activation patching 测试不同位置的功能可迁移性。随后借助 cognitive appraisal theory，将情绪拆解为可解释的 appraisal 概念，并通过因果干预这些概念来改变生成结果。
- **摘要概括**：研究发现情绪信息在模型中的特定区域具有功能定位，并且这种定位在不同模型和规模下具有一定稳健性。对 appraisal 概念进行干预可以按预期改变输出，说明内部情绪表征不仅可被读出，也可以作为敏感情感场景中的控制接口。
- **阅读要点**：该工作体现了“神经表征定位 + 理论先验 + 因果干预”的组合。相比只报告 probe accuracy，它进一步问表征是否符合外部心理学理论，以及改变表征后行为是否随之变化。

## 3. HalluLens: LLM Hallucination Benchmark

- **作者 / 发表**：Yejin Bang, Ziwei Ji, Alan Schelten, Anthony Hartshorn, Tara Fowler, Cheng Zhang, Nicola Cancedda, Pascale Fung；ACL 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.acl-long.1176/)
- **方向**：幻觉诊断、基准测试、外部/内部幻觉分类
- **要解决的问题**：现有工作经常把 hallucination、factuality 和知识错误混在一起，导致不同论文使用的定义、类别和评价方式不一致；尤其缺少专门评估 extrinsic hallucination 的统一基准。
- **怎么解决**：论文提出 HalluLens，先给出区分 intrinsic hallucination 与 extrinsic hallucination 的分类框架，再构造三个面向 extrinsic hallucination 的任务。测试集采用动态生成策略，降低静态数据泄漏并增强评测稳健性，同时发布可复用代码库。
- **摘要概括**：HalluLens 将幻觉从宽泛的“事实性不好”中拆出来，强调生成内容偏离训练知识或用户输入的外部幻觉。它通过统一 taxonomy、动态测试集和多任务设置，为比较不同模型的幻觉行为提供更一致的实验入口。
- **阅读要点**：这类基准论文是可解释性研究的测量基础。没有稳定的行为定义和反泄漏评测，就很难判断一个内部解释是否真的对应幻觉成因，而不是只对应某个数据集的表面模式。

## 4. PrivacyScalpel: Enhancing LLM Privacy via Interpretable Feature Intervention with Sparse Autoencoders

- **作者 / 发表**：Ahmed Frikha, Muhammad Reza Ar Razi, Krishna Kanth Nakka, Ricardo Mendes, Xue Jiang, Xuebing Zhou；BlackboxNLP 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.blackboxnlp-1.13/)
- **方向**：SAE、隐私泄漏、PII 特征干预
- **要解决的问题**：LLM 可能记忆并泄漏个人身份信息；差分隐私或神经元级删除通常会损伤模型效用，或者因为神经元承载多个语义而不能稳定阻断泄漏。
- **怎么解决**：PrivacyScalpel 分三步工作：先用 feature probing 找到编码 PII 的层，再用 k-SAE 分离隐私相关稀疏特征，最后通过 targeted ablation 和 vector steering 在 feature level 进行干预。论文在 Gemma2-2B、Llama2-7B 和 Enron 数据上比较隐私与任务效用。
- **摘要概括**：在实验设置中，方法把 email leakage 从 5.15% 降到 0%，同时保留超过 99.4% 的原始效用，并优于神经元级方法。论文还把隐私防护转化为对 PII 记忆机制的解释性分析。
- **阅读要点**：这里的关键转变是把“解释模型”与“修模型”合并：先找可命名的稀疏特征，再删除或转向这些特征。需要注意的是，隐私比例和效用结果依赖数据、攻击方式与评测协议，不能直接外推到所有模型。

## 5. Evaluation of Attribution Bias in Generator-Aware Retrieval-Augmented Large Language Models

- **作者 / 发表**：Amin Abolghasemi, Leif Azzopardi, Seyyed Hadi Hashemi, Maarten de Rijke, Suzan Verberne；Findings of ACL 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.findings-acl.1087/)
- **方向**：RAG 归因、反事实评估、元数据偏差
- **要解决的问题**：RAG 中让模型把答案归因到来源文档可以增强可验证性，但模型可能因为作者身份等元数据而偏向某些来源，导致 attribution quality 不能真实反映证据使用情况。
- **怎么解决**：作者定义 attribution sensitivity 和 authorship bias 两个维度，显式提供来源作者信息，并用反事实实验改变作者信息，观察三个 LLM 的归因和答案变化。实验分别比较人类撰写与 AI 生成的来源文档。
- **摘要概括**：加入作者信息会让模型的归因质量发生约 3% 到 18% 的变化，模型还可能偏向显式标注为人类作者的来源。结果表明来源文档的元数据会改变模型的信任判断和归因行为，是 RAG 可靠性中的一种新脆弱性。
- **阅读要点**：这篇论文把可解释性从“答案引用了哪段文本”扩展到“模型为什么相信这个来源”。反事实设计很重要，因为它能区分内容证据效应和作者标签效应。

## 6. Exploring Multilingual Probing in Large Language Models: A Cross-Language Analysis

- **作者 / 发表**：Daoyang Li, Haiyan Zhao, Qingcheng Zeng, Mengnan Du；XLLM 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.xllm-1.7/)
- **方向**：多语言 probing、跨语言表征、低资源语言
- **要解决的问题**：LLM probing 研究长期偏向英语，无法说明模型是否以相同方式编码世界上大多数低资源语言的语言结构。
- **怎么解决**：作者在多个开源 LLM 上使用线性分类器 probing，比较不同语言的 probing accuracy、层间趋势以及 probing vector 的相似性，并按高资源/低资源语言进行对照。
- **摘要概括**：高资源语言的 probing 准确率显著高于低资源语言；高资源语言在更深层的准确率提升更像英语，而低资源语言的层间趋势不同。高资源语言之间的表征相似性也更高，低资源语言与它们之间的相似性较低。
- **阅读要点**：这篇工作提醒我们，解释性结论不能只在英语模型和英语任务上验证。跨语言差异既可能来自训练资源，也可能来自模型内部共享表征不足，因此多语言 interpretability 需要单独的覆盖和校准。

## 7. Probing Internal Representations of Multi-Word Verbs in Large Language Models

- **作者 / 发表**：Hassane Kissane, Achim Schilling, Patrick Krauss；MWE 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.mwe-1.2/)
- **方向**：语言学 probing、层级表征、非线性可分性
- **要解决的问题**：Transformer 是否在内部表征中区分不同类型的多词动词，以及这种词汇/句法信息在哪些层表现得最明显，仍缺少细粒度分析。
- **怎么解决**：论文以 BERT 为对象，研究 phrasal verbs（如 give up）和 prepositional verbs（如 look at），在词级和句子级输出上训练 probing classifier，并用 Generalized Discrimination Value 检查类别的几何可分性。
- **摘要概括**：中间层的分类准确率最高，说明相关语言结构在中间层更容易被读出。但 GDV 显示两类表示的线性可分性较弱，而分类器仍能取得高准确率，说明信息可能以非线性方式编码。
- **阅读要点**：这是“probe 能读出信息”与“信息被线性、局部、因果地编码”之间差异的典型例子。高 probe accuracy 不能单独证明模型采用了人类可解释的简单决策规则。

## 8. From Imitation to Introspection: Probing Self-Consciousness in Language Models

- **作者 / 发表**：Sirui Chen, Shu Yu, Shengjie Zhao, Chaochao Lu；Findings of ACL 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.findings-acl.392/)
- **方向**：概念表征 probing、结构因果游戏、表示操控与获取
- **要解决的问题**：关于语言模型是否具有“自我意识”的讨论容易停留在输出拟人化上，缺少可操作定义、内部表征证据和干预实验。
- **怎么解决**：作者基于心理学和神经科学提出语言模型自我意识的实用定义，并细化十个核心概念；使用 structural causal games 建立功能定义，再按 quantification、representation、manipulation、acquisition 四个阶段评估十个模型、可视化表征、尝试操控并进行针对性微调。
- **摘要概括**：模型内部已经出现部分自我意识相关概念的可检测表征，但这些表征目前难以通过正向干预稳定操控；通过针对性 fine-tuning 则可以获得这些概念。
- **阅读要点**：论文的贡献更偏向建立研究协议，而不是证明模型具有类似人类的主观意识。它把模糊概念拆成可测量变量，并区分“已有表征”“能否操控”和“能否通过训练获得”。

## 9. Understanding the Repeat Curse in Large Language Models from a Feature Perspective

- **作者 / 发表**：Junchi Yao, Shu Yang, Jianhua Xu, Lijie Hu, Mengdi Li, Di Wang；Findings of ACL 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.findings-acl.406/)
- **方向**：机制可解释性、SAE、重复生成、feature steering
- **要解决的问题**：重复生成通常用 decoding 策略缓解，但重复现象背后的内部机制缺少解释；仅改变采样并不能回答哪些模型激活真正推动了重复。
- **怎么解决**：论文提出 Duplicatus Charm：先通过 logit analysis 定位与重复相关的层，再用 SAE 提取和激活操控 repetition features。作者构造覆盖 token 级和 paragraph 级重复的数据集，并通过激活、抑制和不同强度 steering 测量特征的影响。
- **摘要概括**：研究识别出与重复生成相关的特征，并显示增强这些特征会诱发重复，关闭它们则可以缓解 Repeat Curse。该流程将“重复”从输出统计现象转化为可以定位、激活和抑制的内部机制。
- **阅读要点**：这是 SAE 从分析工具走向行为干预的代表案例。它也说明特征定位需要与 logit、数据集和多粒度重复指标结合，否则很难区分真正的重复原因与伴随信号。

## 10. Anchored Answers: Unravelling Positional Bias in GPT-2’s Multiple-Choice Questions

- **作者 / 发表**：Ruizhe Li, Yanjun Gao；Findings of ACL 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.findings-acl.124/)
- **方向**：机制可解释性、位置偏置、MLP/attention 干预、logit lens
- **要解决的问题**：GPT-2 在选择题中会过度偏好第一个选项 A，导致预测依赖选项位置而不是内容，损害模型决策的公平性和可靠性。
- **怎么解决**：作者使用 logit lens 追踪导致 A 偏好的中间 logits，分析 MLP 层和值向量以及 attention heads 的贡献。随后更新相关 MLP value vectors，并重校准注意力模式，以最小范围的参数干预削弱 anchored bias。
- **摘要概括**：研究定位了 GPT-2 中与首选 A 偏置相关的内部模块；定向修改这些向量和注意力后，位置偏置得到缓解，并在多个选择题数据集上提升整体准确率。
- **阅读要点**：该工作展示了从失败案例出发的机制分析路径：先构造可重复的偏置，再追踪造成偏置的 logit 来源，最后做局部编辑。它比单纯的 prompt permutation 更接近因果解释，但跨架构泛化仍需要进一步验证。

## 本批小结

- **解释对象的变化**：从输出行为，扩展到层、attention head、MLP value vector、稀疏特征和来源元数据。
- **证据强度的变化**：从相关性 probing，推进到 activation patching、反事实测试、特征激活/抑制和局部参数编辑。
- **应用方向的变化**：解释性已经直接服务于幻觉评测、RAG 可信度、隐私保护、情绪安全和重复生成控制。
- **仍需警惕的限制**：probe accuracy 不等于因果机制；SAE 特征的命名和可分性仍依赖训练与评测设置；单模型、单语言或单数据集上的干预结果不能直接视为普适规律。

