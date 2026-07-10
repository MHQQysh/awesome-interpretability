# 029. RAGTruth: A Hallucination Corpus for Developing Trustworthy Retrieval-Augmented Language Models

- **作者 / venue**：Cheng Niu, Yuanhao Wu, Juno Zhu, Siliang Xu, KaShun Shum, Randy Zhong, Juntong Song, Tong Zhang；ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.acl-long.585/)
- **任务**：RAG 场景下的 hallucination corpus、span-level detection 与 suppression

## 1. Introduction

RAG 能提供外部证据，却不保证模型忠实使用证据。模型仍可能生成 unsupported claim、与 retrieved passage 矛盾的内容，尤其是把多个证据拼接时。已有数据集常只给答案级 label，无法告诉开发者哪一段生成文本出错，也难以训练小模型做细粒度检测。

RAGTruth 的目标是提供自然生成的 RAG hallucination，带 word/span-level annotation，使研究者能分析“哪句话、哪一段 token、属于哪类错误”。

## 2. Dataset 与方法

- 从多个 RAG 任务和主题收集 query、retrieved passages、reference 与 LLM response。
- 人工标注 hallucinated spans，并区分 unsupported、contradictory 等类型。
- 同时提供检测和抑制实验：用 corpus fine-tune Llama-2-13B 做 hallucination detector，或训练/调整模型减少错误 span。
- 关注 subtle hallucination：语法和语气正常，但证据并不支持；这类错误比明显错误更难检测。

## 3. Baseline 与对比

- prompt-based GPT-4 hallucination detection：强 LLM 作为检测器。
- 传统/现成 hallucination detector 与小模型 fine-tuning。
- Llama-2-13B 在 RAGTruth 上 fine-tune，测试小于 GPT-4 的模型能否达到竞争力。
- evident vs subtle hallucination 对照，衡量标注粒度和检测难度。

## 4. Findings

- 高质量 span-level corpus 能让相对较小的 LLM 学会竞争性的 hallucination detection，与 GPT-4 prompt-based 方法相比具有实用性。
- 明显 hallucination 更容易检测；subtle hallucination 的 detection 明显更难，说明 answer-level accuracy 会高估 RAG 可靠性。
- RAGTruth fine-tuning 不只提供分类能力，也支持 hallucination suppression；模型可以用错误 span 作为训练信号降低 unsupported/contradictory generation。
- 数据集的价值在于保留完整 context：query、evidence、response 和标注 span 可以用于研究 retrieval failure 与 generation failure 的边界。

## 5. 局限与复现

人工标注仍有主观性；RAGTruth 的任务分布不能覆盖所有领域、语言和长上下文。检测结果依赖 span 定义、prompt、阈值和评价器。使用时应分别报告 span precision/recall、response-level rate 和 suppression 后的事实性。

**一句话评价**：RAGTruth 把 RAG 幻觉从答案级现象细化到可标注的文本 span，为训练检测器和分析 evidence-grounding 失败提供了基础设施。
