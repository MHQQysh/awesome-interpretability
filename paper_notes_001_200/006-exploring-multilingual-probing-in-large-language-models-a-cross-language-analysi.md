# 006. Exploring Multilingual Probing in Large Language Models: A Cross-Language Analysis

## 6. Exploring Multilingual Probing in Large Language Models: A Cross-Language Analysis

- **作者 / 发表**：Daoyang Li, Haiyan Zhao, Qingcheng Zeng, Mengnan Du；XLLM 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.xllm-1.7/)
- **方向**：多语言 probing、跨语言表征、低资源语言
- **要解决的问题**：LLM probing 研究长期偏向英语，无法说明模型是否以相同方式编码世界上大多数低资源语言的语言结构。
- **怎么解决**：作者在多个开源 LLM 上使用线性分类器 probing，比较不同语言的 probing accuracy、层间趋势以及 probing vector 的相似性，并按高资源/低资源语言进行对照。
- **摘要概括**：高资源语言的 probing 准确率显著高于低资源语言；高资源语言在更深层的准确率提升更像英语，而低资源语言的层间趋势不同。高资源语言之间的表征相似性也更高，低资源语言与它们之间的相似性较低。
- **阅读要点**：这篇工作提醒我们，解释性结论不能只在英语模型和英语任务上验证。跨语言差异既可能来自训练资源，也可能来自模型内部共享表征不足，因此多语言 interpretability 需要单独的覆盖和校准。
