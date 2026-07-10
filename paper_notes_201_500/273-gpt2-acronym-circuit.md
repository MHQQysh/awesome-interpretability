# 273. How Does GPT-2 Predict Acronyms? Extracting and Understanding a Circuit via Mechanistic Interpretability

- **Authors:** Jorge García-Carrasco, Alejandro Maté, Juan Trujillo
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2405.04156
- **Tags:** GPT-2, Acronym Circuit, Attention Heads, Mechanistic Interpretability

## Introduction

GPT-2 能从“The capital of the United States is”这类上下文预测 USA 等缩写，但 token 级输出背后需要识别实体、提取词首字母并拼接。论文选择 acronym prediction 作为可控任务，目标是从行为结果反推一个由 attention heads、MLP 和 residual stream 组成的可理解 circuit。

## Method / Framework

作者设计 clean prompts 与逐阶段 corrupted prompts，先观察单字母、二/三字母预测，再用 activation/path patching 与 logit difference 衡量各组件对最终 acronym token 的增量。电路分析追踪 name/entity mover、letter extraction、composition 和 output heads，并用逐步加入组件的曲线验证最小充分子图；随后把 circuit 中间向量投影到字母 logits，理解每个位置怎样贡献 acronym。

## Baselines / Comparisons

比较包括 full GPT-2、逐步加入的候选 heads/MLPs、corrupted prompt、随机/非候选组件与不同 acronym 长度。logit lens/直接贡献用于初步定位，activation patching 作为 causal check；任务不是与语言模型 SOTA 比准确率，而是比较“候选电路是否保留原行为”。

## Experiments / Findings

研究发现 acronym 预测可被分解为识别相关词、把词首信息搬到合适位置、组合多字母并写入输出 logits 的步骤。逐渐加入 circuit components 时 logit difference 接近 full model，说明任务确实由少量可分析组件驱动，而不是整个模型均匀参与。不同 acronym 长度暴露了电路的复用和 position-specific behavior：第三字母需要额外的上下文/输出路径，不能简单复制第一字母机制。

## Ablation / Error Analysis

prompt corruption 是关键消融：屏蔽实体、字母位置或候选中间 token 会分别破坏对应阶段；只保留 attention 可能无法恢复 MLP 的非线性组合，反之亦然。直接 logit attribution 可能把相关但非必要组件排进 circuit，path patching 和 progressively-added curve 能减少这一问题。少见词形、tokenization 和缩写多义会产生错误。

## Limitations

任务集中在 GPT-2 与英文 acronym，电路可能依赖训练语料中的固定模式；“最小”受所选组件粒度、corruption 和阈值影响。不能据此断言现代 LLM 的所有词法组合都采用同一电路。

## 两句话总结

论文把 GPT-2 的缩写预测拆成实体识别、字母搬运、组合与输出写入的可定位 circuit，并用干预而非仅相关性验证组件贡献。它展示了词法任务中 attention/MLP 协作的具体形态，但电路的最小性和跨模型泛化仍需更多控制实验。

## Evidence note

本笔记逐段核对 arXiv PDF 的 Introduction、circuit extraction、progressive component addition、letter analysis 与 Conclusion。

