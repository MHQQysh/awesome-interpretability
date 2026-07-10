# 195. Bad Actor, Good Advisor: Exploring the Role of Large Language Models in Fake News Detection

- **Authors:** Beizhe Hu, Qiang Sheng, Juan Cao, Yuhui Shi, Yang Li, Danding Wang, Peng Qi
- **Venue:** AAAI 2024
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/30214
- **Tags:** Fake News Detection, Rationale, LLM Advisor, Distillation

## Introduction

假新闻检测同时需要文本线索、常识和外部事实，小型任务模型 BERT 有较好的判别能力但知识有限；GPT-3.5 有更丰富的知识，却不一定能把多角度 rationale 正确整合成真假判断。论文先问“大模型能不能替代小模型”，再把它重新定位为提供分析证据的 advisor。

## Method / Framework

作者用 GPT-3.5 的 zero-shot/few-shot、vanilla/CoT 四种 prompting 生成真假判断和多视角 rationale，发现 rationale 往往比最终 label 更有用。Adaptive Rationale Guidance (ARG) 用 news encoder 编码新闻，用 rationale-aware feature interaction 和 LLM-judgment/usefulness evaluator 选择性注入 text、commonsense 等视角，再由小模型分类；ARG-D 通过 knowledge distillation 学会 rationale-aware feature simulator，推理时不再调用 LLM。

## Baselines / Comparisons

在中文 Weibo21 和英文 GossipCop 上比较 LLM-only、SLM-only、LLM+SLM 组合方法，包括 GPT-3.5 prompt、vanilla BERT、EANNT、Publisher-Emo，以及简单 rationale feature 拼接和 SuperICL。指标主要是 macro-F1/accuracy，并比较 ARG、ARG-D、仅新闻、仅 rationale 和 oracle 多视角投票。

## Experiments / Findings

- GPT-3.5 在两个数据集上整体不如任务微调 BERT，但能给出 commonsense、factuality、textual style 等互补分析；“大模型更大”并不等于真假判断更可靠。
- ARG 的 macro-F1 超过所有比较方法，ARG-D 除 ARG 及其变体外也超过其他方法，说明选择性使用 rationale 比盲目拼接更有效，蒸馏还保留了成本敏感场景中的收益。
- 误差分析显示，多视角 rationales 有助于补足小模型知识，但最优 oracle 投票仍高于 ARG，说明 rationale 选择和融合还有空间；模型 shift 可以在成本和性能之间切换 ARG 与 ARG-D。

## Ablation / Error Analysis

消融去掉 textual/commonsense rationale、LLM judgment predictor、usefulness evaluator 和 attention aggregation。结果表明两类视角都贡献性能，usefulness gating 能避免错误 rationale 反而干扰分类；错误常来自 GPT-3.5 的事实判断失误、情绪化文字误导、rationale 与新闻不相干，以及 temporal split 下知识过时。

## Limitations

GPT-3.5 API 成本和不稳定性使 ARG 难以直接部署，ARG-D 则依赖蒸馏覆盖的分布。两个数据集主要是文本假新闻，不能代表多模态、实时事件和需要外部检索的事实核查；生成 rationale 是辅助证据，不是对小模型决策的 faithful causal explanation。

## 两句话总结

这篇论文发现 LLM 在假新闻检测中更像“懂常识但不会稳定下结论的顾问”，因此用 ARG 选择性把 rationale 交给强判别的小模型，并用 ARG-D 蒸馏掉在线调用。它证明解释信息可以比 LLM 的最终 label 更有价值，但 rationale 的事实性、选择机制和外部事件泛化仍需严格核查。
