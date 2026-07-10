# 439. A Survey of Large Language Models Attribution

- **Authors:** Dongfang Li, Zetian Sun, Xinshuo Hu, Zhenyu Liu, Ziyang Chen, Baotian Hu, Aiguo Wu, Min Zhang
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2311.03731
- **Tags:** `survey` `attribution` `citation` `factuality` `retrieval-augmented-generation`

## Introduction
本文把 LLM attribution 定义为：模型生成一条 claim 时，同时提供可识别、可验证且真正支持该 claim 的 source/citation。它认为 attribution 是降低 hallucination 的现实路径，因为“直接证明回答为真”通常很难，而让用户检查证据更可行。survey 重点不是复述单篇算法，而是梳理来源、任务、方法、数据集、评测和仍未解决的 precision/recall 矛盾。

## Method / Framework
作者把 attribution 的质量拆成 correctness/groundedness、sufficiency/precision 和 comprehensiveness/recall：每条引用都应直接支持对应陈述，并尽量覆盖全部可验证陈述。来源分为 pre-training data 与 out-of-model knowledge；方法分为 direct model-driven attribution、post-retrieval answering、post-generation attribution 和完整 attribution systems。

survey 进一步整理 evaluation：AIS/Auto-AIS、WICE、FActScore、AttrScore、FacTool、KaLMA 等分别从 claim 支持、引用完整性、事实性或自动评估角度测量。它还按数据集总结 WebGPT、ALCE、ExpertQA、HAGRID、SEMQA、BioKaLMA 等，强调“有 retrieval”不自动等于“有 attribution”，因为模型可能仍混入内部知识，或生成引用但引用并不蕴含 claim。

## Baselines / Comparisons
作为 survey，本文没有统一的新模型 baseline，而是横向比较直接引用生成（RECITE、according-to prompting、IFL、1-PAGER）、检索后回答（Self-RAG、RARR、AGREE 等）和生成后检索/归因（RARR、SAPLMA、CCVER 等）。比较维度是 source availability、claim coverage、citation correctness、是否需额外检索、是否支持开放域和是否会引入过量引用。

## Experiments / Findings
核心综合结论是 attribution 同时存在 high recall 与 high precision 的张力：只引用少数最相关段落会漏掉回答中的大量 claim；强行覆盖所有句子又会添加并不支持陈述的 citation。survey 引用的已有结果显示，生成式搜索系统的内容只有约 51.5% 能被其引用完全支持；医学和法律领域还存在大量不完整或不可靠来源，说明“展示了链接”不能替代 groundedness 评估。

直接由模型生成 citation 的方法容易出现 fabricated references 或引用不存在；post-retrieval 方法的优点是证据先可见，但 retrieval 边界与模型内部知识可能混淆；post-generation 方法能够回查并改写答案，却增加延迟且可能只为已有答案寻找事后合理化证据。survey 因而把 attribution 看成从知识来源到 claim、从 claim 到评测的系统链路，而不是一个 prompt 技巧。

## Ablation / Error Analysis
survey 的主要“消融”是 taxonomy 对照：删除 retrieval、改变 attribution 时机、改变 claim 粒度和更换自动 evaluator 都会改变结论。错误可分为 source 不存在/不可靠、source 存在但不支持 claim、只支持部分 claim、citation 覆盖不全，以及模型把内部参数知识误写成外部证据。过量引用也会降低可读性、把不相关页面伪装成支持证据。

作者特别提醒自动评估器本身可能继承 LLM judge 的偏差，单纯 sentence-level 评分也可能掩盖一个句子中多个 claim 的不同支持程度；因此需要 claim segmentation、证据充分性和来源质量的组合评测。

## Limitations
这是 2023 年早期领域综述，方法、基准和模型变化很快；不少结论依赖各论文不同的数据、prompt 和 judge，不能直接当作统一排行榜。survey 也主要围绕文本开放域生成，跨语言、多模态、长文档、动态网页和隐私/版权来源的 attribution 仍不完整。

## 两句话总结
本文用 correctness、precision/sufficiency 和 recall/comprehensiveness 统一描述 LLM 引用的可靠性，并按来源、生成时机、数据集和评测器整理 attribution 研究。它的关键提醒是 retrieval、citation 和 factuality 不是同义词，真正可用的系统必须同时保证每个 claim 被充分且正确地支持，并处理内部知识与外部证据的边界。

## Evidence note
已逐段读取本地 survey PDF，核对了 taxonomy Figure 2、三类 attribution 方法、来源/数据集/评测器分类，以及 51.5% 完全支持率和医学/法律 attribution 缺口等综合结论。
