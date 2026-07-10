# 009. Understanding the Repeat Curse in Large Language Models from a Feature Perspective

## 9. Understanding the Repeat Curse in Large Language Models from a Feature Perspective

- **作者 / 发表**：Junchi Yao, Shu Yang, Jianhua Xu, Lijie Hu, Mengdi Li, Di Wang；Findings of ACL 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.findings-acl.406/)
- **方向**：机制可解释性、SAE、重复生成、feature steering
- **要解决的问题**：重复生成通常用 decoding 策略缓解，但重复现象背后的内部机制缺少解释；仅改变采样并不能回答哪些模型激活真正推动了重复。
- **怎么解决**：论文提出 Duplicatus Charm：先通过 logit analysis 定位与重复相关的层，再用 SAE 提取和激活操控 repetition features。作者构造覆盖 token 级和 paragraph 级重复的数据集，并通过激活、抑制和不同强度 steering 测量特征的影响。
- **摘要概括**：研究识别出与重复生成相关的特征，并显示增强这些特征会诱发重复，关闭它们则可以缓解 Repeat Curse。该流程将“重复”从输出统计现象转化为可以定位、激活和抑制的内部机制。
- **阅读要点**：这是 SAE 从分析工具走向行为干预的代表案例。它也说明特征定位需要与 logit、数据集和多粒度重复指标结合，否则很难区分真正的重复原因与伴随信号。
