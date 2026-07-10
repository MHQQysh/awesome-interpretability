# 040. Investigating Layer Importance in Large Language Models

- **作者 / venue**：BlackboxNLP 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.blackboxnlp-1.29/)
- **任务**：用 Shapley values 和 layer ablation 判断不同 Transformer layer 的功能重要性

## 1. Introduction

LLM 的层数很多，但并不是每一层对每个任务同等重要。只看最终准确率无法知道模型把哪些能力放在早期、中间或后期；只做单层 ablation 又可能把冗余和补偿机制误判为“不重要”。论文将 layer importance 视为一种可量化解释问题。

## 2. Method

- 使用 Shapley value 衡量 layer 对任务性能的边际贡献，考虑不同层组合而非只看单层。
- 做 layer ablation，删除/屏蔽指定 layer，观察性能变化。
- 在多个 NLP tasks 上比较重要层分布。
- 根据高 Shapley layer 设计“关键层”假设，再用 ablation 验证是否真的造成性能下降。

## 3. Baseline 与对比

- random layer ablation：检验重要层效果是否超出随机波动。
- 单层 ablation vs Shapley-based ranking：比较局部删除和组合贡献。
- 不同任务、模型和层位置：避免把一个任务的 layer importance 当成全局规律。
- performance-only analysis 与 attribution-based analysis 对照。

## 4. Findings

- 某些早期 layer 的 Shapley value 较高，删除后任务性能明显下降；这些层可能承担基础 token/representation processing。
- 删除其他 layer 只造成边际变化，说明模型存在冗余或多路径计算。
- 层重要性强烈依赖任务，不能直接声称“最后几层最重要”或“中间层都是语义层”。
- Shapley ranking 与 ablation 结合比单独看相关性更可信：前者提出候选，后者提供干预验证。

## 5. 局限

精确 Shapley 计算成本高，近似采样可能改变排名；layer ablation 会引入分布外 hidden state。重要性也不等同于可解释性：高贡献 layer 不一定对应一个人类可命名概念。

**一句话评价**：论文提供了从“层可视化”走向“层贡献量化 + 干预验证”的路线，强调 layer importance 必须在具体任务和控制实验中定义。
