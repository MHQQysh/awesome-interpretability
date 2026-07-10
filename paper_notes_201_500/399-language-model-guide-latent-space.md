# 399. A Language Model's Guide Through Latent Space

- **Authors:** Dimitri von Rütte, Sotiris Anagnostidis, Gregor Bachmann, Thomas Hofmann
- **Venue / year:** arXiv, 2024
- **Paper:** https://arxiv.org/abs/2402.14433
- **Tags:** `activation-steering` `concept-probing` `representation` `truthfulness` `guidability`

## Introduction
concept guidance 通过在 inference 时沿 activation space 的 concept vector 改变模型行为，比 full fine-tuning 便宜，也有机会直接控制 truthfulness、compliance、humor 等可读概念。但已有工作常只展示单个概念/模型，且“能检测到概念”不等于“能沿该方向可靠引导生成”。本文系统比较 detection 和 guidance 的关系。

## Method / Framework
作者从 OpenAssistant 标注数据和自建 appropriateness 数据中抽取概念标签，在不同层、token position 和模型 activation 上训练三类 linear probes，包括 logistic regression、difference-in-means/PCA 类方向。检测得到 concept vector 后，在生成时施加 activation steering，增强或削弱概念。

为了同时衡量行为效果和语言质量，提出 **perplexity-normalized effect size (PNES)**：把 concept elicitation/guidance 的效果与 perplexity 代价结合。模型包括多个 Llama-family/chat LLM；概念包括 truthfulness、compliance、appropriateness、humor、creativity 和 quality。

## Baselines / Comparisons
比较维度包括 probe 类型、检测层、guidance 层、guidance strength、模型和概念。baseline 是无 steering 或普通 activation guidance；效果以 concept classifier accuracy/elicitation 和 perplexity、输出质量共同评价。

## Experiments / Findings
truthfulness 在多模型上相对 robustly guidable，符合已有工作；humor 需要大量层和强度调参；appropriateness 几乎无法可靠 elicitation，并容易和 compliance 概念混淆。concept guidance 往往能把行为推向目标，但 guidance strength 增大时 perplexity 指数上升，过强会导致 gibberish，存在清晰的 utility-control tradeoff。

最重要的 finding 是 probe detection accuracy 不保证 guidance quality：检测最好用的 probe 不一定是最有效或最自然的 guide。不同概念、模型和 layer 的 PNES 差异很大，因此不能用 truthfulness 的成功经验外推到 humor/appropriateness。

## Ablation / Error Analysis
作者系统消融 probe、层和 strength，比较最佳 PNES 配置与统一配置。不同 guidance strength 形成 Pareto front：提高概念效果通常损伤 fluency。appropriateness 的失败不只是 classifier 弱，而是 model 的 compliance/appropriateness 表征重叠，施加方向会产生 concept confusion。

另一个误差来源是 probe 只读出表征中的线性信息；它可能利用 prompt artifacts 或 label imbalance，且成功 steering 可能是外部向量注入造成的新因果路径，不代表模型原本有一个纯粹的语义模块。

## Limitations
概念定义和 synthetic appropriateness 数据可能带有研究者偏见，自动 classifier 也不是完整 human judgment。activation steering 需要白箱访问，强度和层选择需要手工调优，且过强会降低输出质量。研究覆盖的概念有限，不能证明所有行为都可由一维线性方向控制。

## 两句话总结
本文用多种 linear probes 和 activation steering 系统比较 LLM 概念的 detectability 与 guidability，并用 PNES 衡量行为改变和语言质量的折中。truthfulness 较易控制而 humor/appropriateness 需要精细调参，证明“读得出某概念”并不等于“沿它改得动模型”。

## Evidence note
已读取 arXiv 2402.14433 本地 PDF 的摘要、concept detection/guidance、PNES、Table 1-2 和 layer/probe 消融。
