# 143. Disentangling Transformer Language Models as Superposed Topic Models

- **Authors:** Jia Peng Lim, Hady W. Lauw
- **Venue:** EMNLP 2023
- **Paper:** https://aclanthology.org/2023.emnlp-main.534
- **Tags:** Mechanistic Interpretability, Superposition, Topics, Neurons

## Introduction

Transformer MLP neuron 常有 polysemantic behavior：同一个 neuron 在不同输入中响应不同概念。作者注意到，decoder-only LM 的 neuron logits 与 Neural Topic Model 的 decoder weight 形式相似，因此提出把一个 neuron 看成多个 topic 的线性 superposition，而不是强迫它只有一个语义。

## Method / Framework

对 GPT-2 的每个 residual / MLP neuron，取其投影到 vocabulary 的 logit vector。先按 elimination、mixture、promotion 等 logit 区域构造 token pools，再在词图上用相似 token 聚类，得到候选 word sets。随后把 disentanglement 映射为图问题，用启发式和 exact solving 找到互相分离的 topic sets，最后用 topic coherence 评价。

论文还提出 zero-shot topic modelling（ZSTM）：给定一段 Wikipedia corpus，记录哪些 neuron 被激活，再把它们的 disentangled topics 汇总成文档主题；最后扩展到 LLaMA，并处理其 tokenization 差异。

## Baselines / Comparisons

主要 baseline 是从随机 logit distribution 或随机 path 得到的 word sets，避免把任意聚类都误判成 topic。不同 GPT-2-S、M、L、XL 模型、不同 residual / MLP path 和 topic size K 进行比较；ZSTM 与传统 topic model 的 coherence 及 topic 数量做对照。

## Experiments / Findings

- GPT-2 的神经元确实能被拆成多个相干 topic；GPT-2-sampled word pools 与 random baseline 在 pool size、top-1 NPMI 等分布上有显著差异（Mann-Whitney U，p<0.001）。
- topic examples 通常能对应 Wikipedia 中的可读主题，支持“superposition interpretation 不是随机 mirage”这一主张。
- 不同模型层和不同 logit path 的 topic coherence 有差异，但在 GPT-2 多个尺寸上都能复现；这说明方法不是某一个模型偶然现象。
- ZSTM 可以通过高激活 neuron 自动形成文档 topic，不需要额外训练或修改 GPT-2；LLaMA 扩展显示 tokenization 需要单独处理，不能直接复制 GPT-2 的 token pool。

## Ablation / Error Analysis

随机 distribution、随机 path、不同阈值 τ、topic set size K 和不同 solver 是主要消融。阈值过宽会把 elimination / promotion 混在一起，阈值过窄又会遗漏语义词；K 增大时 coherence 与覆盖率存在折中。错误 topic 常由多义词、形态变体、短 token 或词表中的函数词造成，且 topic coherence 只能衡量词共现，不足以证明 neuron 在模型计算中真的执行该概念。

## Limitations

方法是 weight-based、corpus-agnostic 的候选解释，但主题仍受选择的 corpus 和词表影响。把 neuron topic 当成因果机制还不够，因为只展示 decoder weight 不等于干预 neuron 后行为改变。图优化的 NP-hard 问题依赖启发式，LLaMA 的扩展也没有覆盖现代大模型的全部规模。

## 两句话总结

论文把 transformer neuron 的 polysemanticity 重新表述为多个 topic 在同一 logit 空间中的 superposition，并用图优化将其拆成可读 topic。GPT-2 和 LLaMA 实验显示这些 topic 比随机 baseline 更有 coherence，但它们仍是权重层面的解释，需要因果干预才能成为完整 mechanistic 结论。
