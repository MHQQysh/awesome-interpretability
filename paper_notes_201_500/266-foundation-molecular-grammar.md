# 266. Foundation Molecular Grammar: Multi-Modal Foundation Models Induce Interpretable Molecular Graph Languages

- **Authors:** Michael Sun, Weize Yuan, Gang Liu, Wojciech Matusik, Jie Chen
- **Venue / year:** ICML 2025
- **Paper:** https://arxiv.org/abs/2505.22948
- **Tags:** Molecular Grammar, Multimodal Foundation Model, Chemical Interpretability, Generation

## Introduction

直接生成 SMILES/SELFIES 数据效率低且不易解释；图 grammar 可以用 functional-group/substructure 规则生成分子，但传统 grammar 依赖专家标注或不可解释 heuristic。FMG 的问题是能否让 multimodal foundation model 从分子图像和文本描述中诱导出可读、可验证、数据高效的 molecular graph language。

## Method / Framework

FMG 把分子渲染成图像，让 MMFM（如 GPT-4o 类视觉语言模型）描述 functional groups、motifs 与 motif interactions，再用 prompt learning 执行 clique-tree decomposition。流程先抽取 base cliques，再 merge chemically meaningful cliques、选择 spanning-tree edges 和 root motif，生成单分子的 minimal specialized grammar；多个分子的 grammar 通过 rule counts 汇总成 hierarchical/reusable grammar，按规则频率随机采样新分子。CoT narrative 保存每次选择的理由，LLM tournament 用 Swiss tournament + Bradley-Terry 排序不同 decomposition stories，并与 chemist labels 验证。

## Baselines / Comparisons

作者比较 grammar generative models MHG、DEG、Random Walk、STONED；motif/scaffold VAE JT-VAE、Hier-VAE、MoLeR；chemical language models MolGPT、MolT5、Text+Chem T5；还比较直接 in-context prompting GPT-4。指标包括 validity、uniqueness、novelty、Tanimoto diversity、Retro* success 和 membership，覆盖三个小 monomer 数据集与 HOPV/毒理学真实数据集。

## Experiments / Findings

- 在小数据集上，JT-VAE 等虽然 validity 高，但 unique/novel 可能很低；FMG 在有效性、化学类 membership、diversity 和数据效率之间取得更好的整体折中。直接 GPT-4 ICL 的结构控制不稳定，而 formal grammar 能把 MMFM 的化学判断固化成可执行规则。
- FMG 的优势不是仅来自图 grammar，而来自“图像观察-文字叙事-规则生成”的闭环：设计故事可以解释为什么合并两个 motif、哪个 interaction 重要、哪个 root motif 适合该化学类别。
- 在 ZINC 的 1k subset 上，FMG 在五项 unconditional generation 指标上优于对照，说明 foundation model 的先验可以降低小样本 grammar induction 的数据需求。

## Ablation / Error Analysis

LLM judge/tournament 用来解决不同 decomposition run 的冲突；不做 ranking 或只池化全部 grammar 会把不一致/低质量规则带入生成。prompt 中是否提供上下文也影响 motif 描述：孤立 description 可能只说“benzene”，in-context description 能表达与其他 motif 的连接位置。FMG 倾向形成更复杂、具有化学类别特征的 clique，RHS 越复杂越难复用，导致 coverage/uniqueness 下降；这解释了 validity 很高但多样性不一定最佳的 trade-off。

## Limitations

MMFM 对分子图像的识别依赖渲染规范和视觉/化学先验，LLM judge 不是化学专家且可能偏好语言上漂亮的 story。grammar 的 coverage 在极小数据上仍有限，生成有效不代表可合成、无毒或具备目标药理性质。CoT narrative 是 decision trace，不应直接等同于模型的真实内部化学机制。

## 两句话总结

FMG 用 multimodal foundation model 从分子图像中识别 motif，再通过 clique-tree 和 LLM tournament 诱导出可执行、可读的 molecular grammar。它把大模型的化学先验变成了结构化生成规则并提高小数据效率，但规则覆盖、多样性和化学专家验证仍是主要瓶颈。

## Evidence note

本笔记逐段核对 ICML/arXiv PDF 的 Introduction、FMG induction/inference、Tables 1-2、baseline definitions、case studies 与 Limitations。

