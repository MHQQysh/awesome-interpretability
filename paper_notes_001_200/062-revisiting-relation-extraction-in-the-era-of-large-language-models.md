# 062. Revisiting Relation Extraction in the era of Large Language Models

- **作者 / venue**：ACL 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.acl-long.868/)

## Introduction / Method

论文比较 supervised RE 与 GPT-3/Flan-T5 的 few-shot、CoT 和 fine-tuning。关系抽取要求识别实体及关系，生成式模型还必须把结果线性化成可解析格式，因此评价不能只看 fluent output。

## Baseline

比较标准 supervised RE、REBEL 等 generative baseline、GPT-3 few-shot、Flan-T5 large 和 fine-tuned models；使用 standard RE datasets 与 micro-F1。

## Findings

GPT-3 few-shot 在标准 RE 上接近甚至超过部分 supervised baseline；加入 CoT reasoning 可进一步改善 few-shot 表现。Flan-T5 large 的能力不如 GPT-3 稳定，但 fine-tuning 后可达到 SOTA。生成式 RE 的主要错误来自实体边界、关系格式和推理链不一致。

## 局限

few-shot 结果受 prompt 和 label verbalization 影响；生成文本的格式错误可能被当作语义错误。LLM 的推理解释也需要结构化验证。

**一句话评价**：论文说明 LLM 可以做 RE，但必须同时评估抽取正确性、格式可解析性和 reasoning faithfulness。
