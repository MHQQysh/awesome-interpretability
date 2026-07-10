# 019. Sui Generis: Large Language Models for Authorship Attribution and Verification in Latin

- **作者 / venue**：Michele Di Buono, Francesco Mambrini, Paolo Torroni；NLP4DH 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.nlp4dh-1.39/)
- **任务**：拉丁语 Patristic texts 的 authorship attribution 与 verification

## 1. Introduction：低资源作者识别中的解释性

传统 authorship attribution 依赖大量标注文本和稳定的 stylistic features；对古典拉丁语而言，作者候选少、文本短、题材和体裁混杂，训练数据很有限。LLM 的 zero-shot 能力给了一个新选择，但作者归属不是普通分类：系统必须区分文体、主题、时代和作者身份，并且解释为什么选择某个作者。

论文研究 GPT-4o、Claude、Mistral-Large、Gemini-1.5 在 authorship attribution/verification 中的表现，重点分析 prompt 设计、候选作者数量和解释的一致性。

## 2. Method 与实验协议

- **任务一：attribution**，从候选作者中选择文本作者。
- **任务二：verification**，判断文本是否由给定作者写作。
- 使用直接 prompting，不进行针对古典拉丁语的额外训练。
- 测试三种信息设置：BASE（一般任务描述）、LIMITED（带有限主题/作者信息）和 HIP（historically informed prompting，加入领域专家建议的历史语言学特征）。
- 评价 accuracy/precision、跨 prompt 的 agreement，以及错误案例中的解释文本。

## 3. Baseline 与比较

- **传统 stylometric baseline**：依赖词汇、句法、函数词或 n-gram 特征的作者识别方法。
- **不同 LLM**：GPT-4o、Claude、Mistral-Large、Gemini-1.5；attribution 与 verification 的模型覆盖略有不同。
- **prompt baseline**：generic prompt、LIMITED prompt 与 HIP prompt。
- **候选数量**：比较 5/10 等不同候选作者规模，测试候选空间扩大是否使 LLM 只依赖表面风格。

## 4. Findings：LLM 能做什么、不能做什么

- LLM 在 zero-shot authorship verification 上表现出价值，尤其在短文本和训练数据稀缺的条件下；这支持其作为低资源辅助工具，而不是必须 fine-tune 一个大分类器。
- 显式、领域化指令通常提高 precision 和跨模型 agreement；LIMITED/HIP prompt 比 generic prompt 更容易让模型遵循作者风格证据。
- 但模型在区分“主题内容”和“写作风格”时仍困难：同一作者写不同主题，或不同作者写相近主题时，LLM 可能把内容词汇误当作者特征。
- 解释文本看起来往往合理，但错误案例说明它会事后描述句法复杂度、从句数量等表面特征，未必是模型真正使用的决策依据。

## 5. 解释性与错误分析

论文的重点不是仅报告 accuracy，而是通过 prompt agreement 和案例分析观察解释稳定性。若同一个文本在不同 prompt 下得到不同作者和不同理由，说明 attribution 结论不稳健。历史信息提示能提升结果，但也可能把专家先验变成新的偏置来源。

## 6. 局限

数据规模和作者覆盖有限，拉丁文本有强烈时代、体裁和主题因素；zero-shot 结果可能受模型预训练语料污染。未来需要作者盲测、跨体裁验证和与可解释 stylometry 的严格对照。

**一句话评价**：Sui Generis 说明 LLM 可以帮助低资源 authorship verification，但解释必须经过跨 prompt、跨模型和主题/文体解耦检验，不能因为理由写得像语言学分析就视为可靠。
