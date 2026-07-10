# 002. Mechanistic Interpretability of Emotion Inference in Large Language Models

## 2. Mechanistic Interpretability of Emotion Inference in Large Language Models

- **作者 / 发表**：Ala N. Tak, Amin Banayeeanzade, Anahita Bolourani, Mina Kian, Robin Jia, Jonathan Gratch；Findings of ACL 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.findings-acl.679/)
- **方向**：机制可解释性、情绪表征、activation patching、情感安全
- **要解决的问题**：LLM 能从文本推断人类情绪，但模型究竟通过哪些内部区域处理情绪刺激，以及这些表征是否具有心理学意义，并不清楚。
- **怎么解决**：作者在多个模型家族和规模上进行 emotion probing，检查情绪表征的层级位置，并用 activation patching 测试不同位置的功能可迁移性。随后借助 cognitive appraisal theory，将情绪拆解为可解释的 appraisal 概念，并通过因果干预这些概念来改变生成结果。
- **摘要概括**：研究发现情绪信息在模型中的特定区域具有功能定位，并且这种定位在不同模型和规模下具有一定稳健性。对 appraisal 概念进行干预可以按预期改变输出，说明内部情绪表征不仅可被读出，也可以作为敏感情感场景中的控制接口。
- **阅读要点**：该工作体现了“神经表征定位 + 理论先验 + 因果干预”的组合。相比只报告 probe accuracy，它进一步问表征是否符合外部心理学理论，以及改变表征后行为是否随之变化。
