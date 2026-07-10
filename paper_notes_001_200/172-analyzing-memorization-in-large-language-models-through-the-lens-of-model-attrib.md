# 172. Analyzing Memorization in Large Language Models through the Lens of Model Attribution

- **Authors:** Tarun Ram Menta, Susmit Agrawal, Chirag Agarwal
- **Venue:** NAACL 2025
- **Paper:** https://aclanthology.org/2025.naacl-long.535
- **Tags:** Memorization, Attribution, Attention, Generalization

## Introduction

LLM memorization 可能造成隐私和版权风险，但多数工作只做抽取或统计，很少分离 transformer 哪些 architectural modules 负责 memorization、哪些保留 generalization / reasoning。论文用 model attribution 对 attention block 做干预。

## Method / Framework

在 Pythia、GPT-Neo 等模型中 bypass 指定 block 的 attention，同时保留 layer norm、MLP 等其它变换，比较 intervention 前后的 memorization 与 generalization。作者给出干预输出差异的理论界，并用五个 benchmark 衡量 factual recall、训练数据记忆和推理能力。

## Baselines / Comparisons

比较原始模型、不同层 attention bypass、连续层 bypass、MLP / attention 对照和随机层干预。指标包括 memorized sample exposure、held-out generalization、reasoning / downstream accuracy、输出概率变化与计算成本。

## Experiments / Findings

- 深层 transformer block 的 attention 对 memorization 贡献最大；绕过它们能降低训练数据复现。
- 早期 block 对 generalization 和 reasoning 更关键，直接移除早期 attention 更容易损害正常任务性能。
- 这一分工在 Pythia 与 GPT-Neo 家族中都得到支持，但层位置随模型规模变化。
- 通过选择性 bypass，可以在降低 memorization 的同时尽量保留通用能力，为 inference-time privacy mitigation 提供方向。

## Ablation / Error Analysis

消融层深度、连续 block 数、attention vs MLP、模型家族和数据类型。只看单层 attribution 会忽略跨层协作；过度 bypass 深层会损害 factual knowledge，过度 bypass 早层则损害语义 / reasoning。理论 bound 能解释输出差异上限，但不自动保证隐私消除。

## Limitations

实验模型和 benchmark 有限，memorization 定义依赖 prompt 与暴露测试。bypass attention 需要白盒访问并可能改变 latency；深层 attention 相关不等于唯一 causal source。不能直接推出闭源 LLM 或真实个人信息场景的安全保证。

## 两句话总结

论文从架构干预角度发现深层 attention 更偏向 memorization，而早层组件更支撑泛化和 reasoning。选择性 bypass 提供了隐私与能力折中，但仍需更真实的训练数据、更多模块因果实验和部署评估。
