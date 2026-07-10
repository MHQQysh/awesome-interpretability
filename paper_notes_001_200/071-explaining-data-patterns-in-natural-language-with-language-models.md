# 071. Explaining Data Patterns in Natural Language with Language Models

- **作者 / venue**：BlackboxNLP 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.blackboxnlp-1.3/)

## Introduction / Method

LLM 不只可以预测数据标签，也可以把数据中发现的 pattern 转成自然语言。iPrompt 通过 prompt induction 从样本中恢复 dataset description，并要求提示既能提高预测准确率，又能被人理解。

## Baseline 与对比

比较人工 prompt、AutoPrompt 和 iPrompt，在 ANLI 等数据集与不同规模 GPT/LM 上测试；human evaluation 评价 prompt 是否准确解释数据规则，generalization accuracy 评价生成 prompt 对新模型/数据的迁移。

## Findings

iPrompt 的自然语言解释分数高于基础方法，同时能生成更可迁移的 prompt。较大模型上的 iPrompt 与 AutoPrompt 差距更明显，说明自然语言 pattern recovery 可以成为数据理解工具。

## 局限

prompt 可能描述相关模式而非真正生成机制；人评会偏好流畅表述。解释应与反事实样本、held-out generalization 和 feature ablation 联合验证。

**一句话评价**：iPrompt 让 LLM 将数据规律“说出来”，连接了 prompt discovery、dataset understanding 和可读解释。
