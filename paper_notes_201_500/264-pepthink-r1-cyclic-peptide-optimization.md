# 264. PepThink-R1: LLM for Interpretable Cyclic Peptide Optimization with CoT SFT and Reinforcement Learning

- **Authors:** Ruheng Wang, Hang Zhang, Trieu Nguyen, Shasha Feng, Hao-Wei Pang, Xiang Yu, Xiao Li, P. Zhang
- **Venue / year:** 2025 preprint
- **Paper:** https://arxiv.org/abs/2508.14765
- **Tags:** Molecular Design, Cyclic Peptide, CoT SFT, Reinforcement Learning, Interpretability

## Introduction

治疗性 cyclic peptide 设计面对巨大的 sequence space、稀缺实验数据和多目标冲突，传统生成器可以产生分子，却很难解释为何选某个 monomer modification。PepThink-R1 的目标是把“生成序列”和“优化 lipophilicity、stability、exposure”的多目标控制结合起来，并让每个修改步骤有可读的 reasoning trace。

## Method / Framework

模型先用 CoT supervised fine-tuning 学会以 monomer-level modification 组织 peptide design reasoning，再用 reinforcement learning 优化一个平衡 chemical validity 与 pharmacological property improvement 的 reward。prompt 把目标属性和当前 cyclic peptide 结构交给模型，模型输出修改链与新 SMILES；CoT-SFT-RL 是最终 PepThink-R1，SFT-RL 则使用没有 CoT 的训练模板，用于隔离“有 reasoning supervision”与“只有 RL”的区别。

## Baselines / Comparisons

比较包括 random mutation、GPT-4o、GPT-5、CoT-SFT、SFT-RL、DeepSeek-R1-Llama-8B、Qwen3-30B-thinking，以及 domain-specific peptide optimizer/PepINVENT。结构指标为 validity、novelty、uniqueness、diversity；性质指标包括 HQSR、UHQS 和 HQSR-S，分别反映同时达到 LogD、MRT 和 SIF half-life thresholds 的比例、质量/多样性和 seed success。

## Experiments / Findings

- PepThink-R1 的 HQSR 为 0.890，HQ-Unique/Seed 为 4.42/10 与 11.81/50，HQ-Seed Success 为 0.984；这明显高于 GPT-4o 的 HQSR 0.043 和 CoT-SFT 的 0.197。
- SFT-RL validity 0.987、SFT 0.956，说明监督和 RL 能保持结构有效性；几乎所有方法 novelty >0.98，但 RL 的 uniqueness 下降到 0.2-0.3，反映优化压力会牺牲多样性。
- one-shot prompt 反而普遍降低性能，表明额外示例会干扰模型已有的分子生成逻辑。案例中模型通过 N-methylation、疏水侧链替换、非天然氨基酸等单体级选择解释属性变化，增强了设计可读性。

## Ablation / Error Analysis

CoT-SFT-RL 对照 SFT-RL 是核心消融：相同 RL 框架下，显式 reasoning supervision 带来更高 property control。去掉 CoT 或改为 one-shot 会降低 HQSR；提高 RL 压力又会导致 mode collapse/uniqueness 下降。GPT-4o/5 的自由文本建议通常化学上合理但属性满足率低，说明“通用化学常识解释”与“可验证的多目标优化”不同。

## Limitations

reward 依赖 QSAR/预测器，不能替代真实实验；高 HQSR 可能伴随结构多样性下降。CoT 是模型生成的设计叙事，不保证反映真实内部计算；cyclic peptide 数据和四个 seed case 也不足以证明药物发现有效性。论文需要进一步外部合成验证、毒性评估和多样性-性质 Pareto 分析。

## 两句话总结

PepThink-R1 用 CoT SFT 学会逐单体解释设计，再用 RL 控制多种药理性质，显著提高 cyclic peptide 的联合属性满足率。它的代价是 RL 可能压低结构多样性，且生成的 reasoning 仍是预测器驱动的设计 rationale，不等于实验因果证据。

## Evidence note

本笔记逐段核对预印本 PDF 的 Introduction、Methodology、Tables 2-7、case studies 与 Conclusion；指标和阈值按正文保留。

