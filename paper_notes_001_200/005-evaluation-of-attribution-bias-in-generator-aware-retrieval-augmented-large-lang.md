# 005. Evaluation of Attribution Bias in Generator-Aware Retrieval-Augmented Large Language Models

## 5. Evaluation of Attribution Bias in Generator-Aware Retrieval-Augmented Large Language Models

- **作者 / 发表**：Amin Abolghasemi, Leif Azzopardi, Seyyed Hadi Hashemi, Maarten de Rijke, Suzan Verberne；Findings of ACL 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.findings-acl.1087/)
- **方向**：RAG 归因、反事实评估、元数据偏差
- **要解决的问题**：RAG 中让模型把答案归因到来源文档可以增强可验证性，但模型可能因为作者身份等元数据而偏向某些来源，导致 attribution quality 不能真实反映证据使用情况。
- **怎么解决**：作者定义 attribution sensitivity 和 authorship bias 两个维度，显式提供来源作者信息，并用反事实实验改变作者信息，观察三个 LLM 的归因和答案变化。实验分别比较人类撰写与 AI 生成的来源文档。
- **摘要概括**：加入作者信息会让模型的归因质量发生约 3% 到 18% 的变化，模型还可能偏向显式标注为人类作者的来源。结果表明来源文档的元数据会改变模型的信任判断和归因行为，是 RAG 可靠性中的一种新脆弱性。
- **阅读要点**：这篇论文把可解释性从“答案引用了哪段文本”扩展到“模型为什么相信这个来源”。反事实设计很重要，因为它能区分内容证据效应和作者标签效应。
