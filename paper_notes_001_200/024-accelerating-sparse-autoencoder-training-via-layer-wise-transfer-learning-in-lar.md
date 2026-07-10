# 024. Accelerating Sparse Autoencoder Training via Layer-Wise Transfer Learning in Large Language Models

- **作者 / venue**：Davide Ghilardi, Federico Belotti, Marco Molinari, Jaehyuk Lim；BlackboxNLP 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.blackboxnlp-1.32/)
- **任务**：降低 SAE 训练成本；利用相邻 Transformer layer 的共享表示初始化 SAE

## 1. Introduction

SAE 已成为研究 superposition 和 monosemantic feature 的重要工具，但对每一层独立从头训练 SAE 的成本很高，尤其是大模型、多层和大 token corpus。相邻层的 activation 分布并非完全相同，却可能共享部分结构；如果这种共享足够强，就可以迁移邻层 SAE 权重，减少收敛时间。

## 2. Method

- **Scratch-SAE baseline**：每层独立随机初始化并训练 SAE。
- **Fwd-SAE**：使用前一层训练好的 SAE 权重初始化当前层。
- **Bwd-SAE**：反向使用后一层 SAE 权重初始化当前层。
- 对迁移后的 SAE 继续 fine-tune，而不是直接复用不匹配的 dictionary。
- 评估 reconstruction/L2 loss、L0 sparsity、cross-entropy loss 和特征质量，并比较不同 checkpoint 的收敛曲线。

核心假设是：相邻层共享 representation geometry，因此 transfer learning 既能加快优化，也可能给 SAE 更好的起点。

## 3. Baseline 与对比

- 每层从 scratch 训练的 SAE 是最重要对照。
- Fwd 与 Bwd 方向互相对照，用来检验 layer direction 是否影响迁移质量。
- 低层到高层和高层到低层的迁移成本、loss 曲线和最终表示质量分别报告。
- 论文还讨论不同模型家族/架构之间能否迁移，避免把相邻层结果误认为跨模型泛化。

## 4. Findings

- 迁移初始化显著加快 SAE convergence，同时最终 reconstruction 和 sparsity 通常不劣于 scratch baseline。
- 相邻层的共享表示确实足以提供有用初始化，但距离过远或表示分布变化较大的层，transfer benefit 会下降。
- Fwd/Bwd 不一定完全对称：前一层到后一层的迁移更符合逐层表示演化，但反向迁移在部分设置也能工作。
- 这说明 SAE 的训练成本中有一部分来自重复学习相邻层共有结构，而不全是每层都必须重新发现的 feature dictionary。

## 5. 评价与局限

训练加速不等于解释质量自动提高；需要同时检查 feature interpretability、dead features、sparsity 和 downstream intervention。不同层的 feature index 也不能直接认为语义相同。跨模型 transfer 仍需要更系统的对齐和字典匹配。

**一句话评价**：论文把 SAE 训练看成可迁移的优化问题，说明层间表示共享不仅是解释对象，也可以直接用于降低 mechanistic interpretability 的实验成本。
