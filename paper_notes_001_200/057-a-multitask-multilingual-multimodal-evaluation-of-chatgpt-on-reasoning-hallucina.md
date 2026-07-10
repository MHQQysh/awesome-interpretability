# 057. A Multitask, Multilingual, Multimodal Evaluation of ChatGPT on Reasoning, Hallucination, and Interactivity

- **作者 / venue**：IJCNLP 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.ijcnlp-main.45/)

## Introduction 与方法

ChatGPT 的对话界面让用户可以追问、纠错和交互，但它的 reasoning、hallucination、multilingual、multimodal 能力需要统一公开评测。论文使用 23 个数据集、8 类任务，覆盖 QA、推理、摘要、翻译、情感、语言识别、对话和 misinformation，并加入视觉/图像交互。

## Baseline 与对比

与公开 supervised SOTA、zero-shot multilingual LLM 和较早模型比较；评估 ChatGPT 在高资源、中资源、低资源语言的差异。多模态任务中，文本模型与图像输入能力也要分开看。

## Findings

ChatGPT 在英语和高资源语言任务上表现强，但低资源语言与复杂推理仍有明显差距。它可以通过对话交互改正部分事实/翻译错误，但也可能在反复追问中生成看似合理的错误。多模态任务显示文本 LLM 的推理能力不能直接外推到视觉内容。

## 局限

模型版本和 prompt 工程会影响结果；公共 benchmark 可能被训练数据污染；交互式评价需要固定轮次和用户策略。

**一句话评价**：这篇工作提供了 ChatGPT 早期能力的多维基线，提醒解释性研究必须把交互纠错、语言资源和模态差异纳入评测。
