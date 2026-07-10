# 259. Understanding the Uncertainty of LLM Explanations: A Perspective Based on Reasoning Topology

- **Authors:** Longchao Da, Xiaoou Liu, Jiaxin Dai, Lu Cheng, Yaqing Wang, Hua Wei
- **Venue / year:** COLM 2025
- **Paper:** https://arxiv.org/abs/2502.17026
- **Tags:** Explanation Uncertainty, Reasoning Topology, Graph Edit Distance, Faithfulness

## Introduction

现有 LLM uncertainty quantification 多比较多个答案的语义差异或 token/logit，不知道不确定性在中间哪一步产生。线性 CoT 也会把并行子任务、依赖关系和逻辑跳跃压扁成一段文字。论文的问题是能否把 explanation 显式结构化为 reasoning topology，从而同时度量语义变化、图结构变化、有效路径和冗余。

## Method / Framework

Topo-UQ 有两个模块。Topology elicitation 先让 LLM 从问题反思必要 knowledge points，把每个点写成 sub-question；再自回答这些问题，最后把 knowledge-answer pairs 连接成从 nodeRaw 到 nodeResult 的 directed graph。Topology-based UQ 对多次生成的图做 semantic embedding，并改造 Graph Edit Distance：node/edge 的替换、删除/插入既考虑 embedding 语义代价，也考虑结构代价，得到 Reason-GED。另一个副产物是 valid reasoning path redundancy：寻找没有连接最终答案的节点/边，量化解释冗余。

## Baselines / Comparisons

实验覆盖 GSM8K、BoolQ、GeoQA，模型包括 GPT-4o-mini、DeepSeek-R1、Llama-3.3-70B、Llama-3-8B 和 Phi-4。基线是 answer/CoT-based UQ (CoTA)、embedding UQ、entailment UQ、NLI-logit UQ；指标是 UQ 分数与 ground-truth faithfulness 的 Pearson、Spearman 和 Kendall 相关，越负越好，因为 faithfulness 越高应对应越低 uncertainty。

## Experiments / Findings

- BoolQ 上，Ours 在 GPT-4o-mini 为 PCC/SRC/KR -0.03/-0.05/-0.03，在 DeepSeek-R1 为 -0.29/-0.26/-0.17，在 Llama-3.3-70B 为 -0.24/-0.20/-0.17；多数优于 baseline，但 Phi-4 接近 0，表明 topology elicitation 失败时方法也会失效。
- GSM8K 上，Ours 对 GPT-4o-mini 为 -0.35/-0.34/-0.23，对 DeepSeek-R1 为 -0.22/-0.20/-0.14，对 Llama-3.3-70B 为 -0.43/-0.41/-0.28；GeoQA 中 GPT-4o-mini 为 -0.22/-0.34/-0.31，DeepSeek-R1 为 -0.29/-0.31/-0.19。结果支持 reasoning-aware UQ 比只看答案更能暴露中间不一致。
- 拓扑聚类得到三类 reasoning pattern：completion/memorization-based、forward reasoning、以及 memory + forward + verify。GPT-4o-mini 的冗余率反而较高，可能通过更大的搜索空间换取准确率；DeepSeek-R1 的有效路径更紧凑。

## Ablation / Error Analysis

Topology elicitation 是关键消融点：较小模型或 instruction-following 弱的模型可能不能稳定地产生节点、边和结果连接，导致 UQ 相关性下降。图距离只看结构会丢失节点语义，纯 embedding/entailment 又会忽略逻辑依赖；Reason-GED 的收益来自二者结合。方法也需要多次生成并两两计算图距离，若有 n 个 response、每图 m 个节点，复杂度约为 O(n²m³)。

## Limitations

方法依赖强 instruction following 和额外 elicitation，比较适合大模型/长上下文；它验证的是 explanation faithfulness，不直接保证最终 answer correctness。拓扑由 LLM 自己构造，可能把“看起来像推理”的图当作真实内部计算；冗余率也不等于推理质量。作者还需要在更复杂任务和更少 API 调用下验证。

## 两句话总结

Topo-UQ 把自然语言解释拆成 knowledge-point 图，用结合语义与结构的 Reason-GED 度量解释不确定性，并从有效路径中测冗余。它比 answer-only UQ 更能定位中间推理差异，但图是模型外显的 proxy，不是内部电路的直接观测，且需要较强的指令遵循能力。

## Evidence note

本笔记逐段核对 COLM/arXiv PDF 的 Definitions、Method、Table 1/4、reasoning-pattern analysis、complexity 和 Conclusion；相关系数与复杂度来自正文。

