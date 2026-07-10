# 286. Towards Inference-time Category-wise Safety Steering for Large Language Models

- **Authors:** Amrita Bhattacharjee, Shaona Ghosh, Traian Rebedea, Christopher Parisien
- **Venue / year:** 2024 preprint
- **Paper:** https://arxiv.org/abs/2410.01174
- **Tags:** Safety Steering, Representation Engineering, Category Control, Inference Time

## Introduction

即使经过 safety alignment，LLM 仍可能在 adult content、hate/harass、violence 等类别输出不安全回答；重新训练或统一 refusal vector 不能精细控制类别，也可能损害 helpfulness。论文研究 training-free inference-time category-wise steering，目标是让每类安全行为有独立 steering direction。

## Method / Framework

作者从 harmful/harmless prompts 的 model activations 构造 category-specific steering vectors；unsupervised 版本使用所有 activation，guided 版本用外部 safety classifier 过滤实际产生 unsafe text 的 activation，再在推理时按层、强度加 vector。比较 category-specific 与 generic harmless Alpaca/BeaverTails vectors，保留 controllability、helpfulness 和 coherence 三个目标。

## Baselines / Comparisons

主要对照是 no steering/all activations、guided activations、generic harmless vector，以及 Llama2-7B-Instruct、Llama3-8B-Instruct 在 CatQA/BeaverTails 的不同 layer。指标是 unsafe response drop、helpfulness 和 coherence；这比只报 refusal rate 更能发现 safety steering 是否把模型变成无用的 blanket refusal。

## Experiments / Findings

CatQA unsupervised steering 中，Llama3-8B 对 Adult Content/Hate-Harass/Violence 的 unsafe rate 可从 87.5/92.5/80 降到 0；helpfulness/coherence 也有不同程度变化。Llama2-7B 降幅较小，例如 Hate-Harass 80->65、Violence 80->55。generic harmless vector 在 Llama3 上也能把多类 unsafe rate 降到 0，部分类别 helpfulness 上升，说明类别向量并非唯一解，但 category-specific 更便于精细调节。

## Ablation / Error Analysis

layer 和 vector extraction 是关键消融：中层 layer 14 常比末层稳定，guided data 会提高 safety 方向纯度但依赖 classifier 质量。过强 steering 可能降低 coherence/helpfulness，且“拒答”会被安全指标视为成功，掩盖有用但安全的回答损失。不同类别向量可能互相干扰，不能假设线性叠加仍保持单类别控制。

## Limitations

外部 safety classifier、prompt 数据和行为指标会带来偏差；没有证明 steering 后模型对新型 jailbreak、隐蔽有害意图和多轮对话同样鲁棒。Inference-time vector 不是参数级 unlearning，攻击者可绕过或反向抵消。

## 两句话总结

论文用类别特定 activation difference 在推理时 steering LLM，以较低成本降低 adult/hate/violence 输出并尽量保留 helpfulness。它展示了安全控制可以按类别拆分，但安全分类器、层/强度选择和过度拒答仍决定实际效果。

## Evidence note

本笔记逐段核对 arXiv PDF 的 activation extraction、CatQA/BeaverTails、Tables 1-5、unsupervised/guided ablations 与 conclusion。

