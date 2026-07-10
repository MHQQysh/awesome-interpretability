# 001. Evaluating Sparse Autoencoders for Monosemantic Representation

## 1. Evaluating Sparse Autoencoders for Monosemantic Representation

- **作者 / 发表**：Moghis Fereidouni, Muhammad Umair Haider, Peizhong Ju, A.B. Siddique；Findings of EACL 2026
- **论文链接**：[ACL Anthology](https://aclanthology.org/2026.findings-eacl.313/)
- **方向**：SAE、monosemanticity、概念级干预
- **要解决的问题**：SAE 常被认为可以缓解大模型神经元的 polysemanticity，但此前缺少把 SAE 与原始模型进行统一、定量比较的指标；同时，特征分解是否真的带来更精确的概念控制，也需要直接验证。
- **怎么解决**：论文提出基于 Jensen-Shannon 距离的 concept separability score，从激活分布角度比较原始模型和 SAE 特征。作者在 Gemma-2-2B、DeepSeek-R1、多种 SAE 和五类数据上评估，并比较完整 masking、部分 suppression 以及新提出的 APP（Attenuation via Posterior Probabilities）干预。
- **摘要概括**：实验显示 SAE 特征具有更高的概念可分性，能够降低多义性；在概念删除任务中，部分抑制比整神经元屏蔽更精确。APP 利用概念条件激活分布进行定向抑制，在有效删除概念的同时带来最小的困惑度上升。
- **阅读要点**：这篇工作把“SAE 特征更可解释”的定性判断推进为分布距离和干预效果两个可量化标准。它的价值不只在 feature discovery，也在于把解释单元直接连接到了模型编辑与控制。
