# 248. Large Language Models Hallucination: A Comprehensive Survey

- **Authors:** Aisha Alansari, Hamzah Luqman
- **Venue:** arXiv 2026 survey
- **Paper:** https://arxiv.org/abs/2510.06265
- **Tags:** Hallucination, Detection, Mitigation, Survey

## Introduction

LLM hallucination 是流畅但事实错误或缺乏证据的输出，在知识问答、医疗、法律和 agent 场景中直接影响可靠性。综述试图统一解释 hallucination 的类型、生命周期根因、检测方法、缓解方法、benchmark 和 metrics，而不是将所有错误都称为同一种幻觉。

## Method / Framework

作者按 data collection、architecture、training、decoding/inference 的生命周期分析原因，构建 hallucination taxonomy；再按 intrinsic/extrinsic、factuality/faithfulness 等分类 detection，按 retrieval、verification、decoding、training/alignment、self-correction 等分类 mitigation，最后整理评测 benchmark 与指标。

## Baselines / Comparisons

综述横向比较 model-based/black-box detector、uncertainty/probability、retrieval/evidence verification、LLM judge、self-consistency 和 human evaluation。重点比较 detection precision/recall、faithfulness、事实覆盖、计算成本、领域泛化和 mitigation 后 helpfulness trade-off。

## Experiments / Findings

- hallucination 根因横跨数据缺失、参数记忆、训练目标、上下文冲突、解码随机性和 prompt/agent pipeline，不能只用更大模型解释。
- 现有检测方法在开放世界、长答案和多语言场景缺少统一 ground truth；缓解方法往往降低 hallucination 却增加拒答、延迟或损失信息量。
- 综述强调 benchmark 与 metric 需要把事实正确、证据支持、任务完成和解释忠实分开，未来应结合外部证据、过程监控与机制特征。

## Ablation / Error Analysis

survey 不提出新统一 ablation；其方法学警告是 detector/LLM judge 可能共享模型偏差，retrieval 可能引入错误证据，self-correction 可能重复同一错误。单一 prompt、单一 judge 或单一 dataset 的提升不应当作通用 hallucination 解决。

## Limitations

作为快速更新的综述，taxonomy 和 benchmark 会随新模型变化；不同论文的 hallucination 定义、标注和指标难以直接比较。开放世界事实、文化/语言差异、multimodal/agent hallucination 仍需要更统一评测。

## 两句话总结

综述从生命周期根因、类型、检测、缓解和 benchmark 五条线梳理 LLM hallucination，强调可靠性不是单一 factuality 分数。它的价值是统一比较框架，但真正进展仍取决于可验证证据、过程/机制监控和跨领域评测。
