# 477. Interpretable Multimodal Sentiment Analysis Based on Textual Modality Descriptions

- **Authors:** Sixia Li, Shogo Okada
- **Venue / year:** arXiv 2023
- **Paper:** [arXiv:2305.06162](https://arxiv.org/abs/2305.06162)
- **Tags:** `multimodal-sentiment` `natural-language-explanation` `modality-fusion`

## Introduction
多模态 sentiment 模型通常把语言、音频和脸部信号拼成向量，准确率可以提高，却难以告诉人类每个 modality 如何影响 sentiment。作者把非语言信号转换成自然语言描述，再用 BERT/RoBERTa/ChatGPT 处理，以获得可读的 modality-level evidence。

## Method / Framework
音频被映射成 pitch/energy 等描述，facial action units 被改写成自然语言动作描述；语言、audio、face 的描述以 separator 或 paragraph 组合，输入 frozen/fine-tuned BERT/RoBERTa，并比较单模态与组合模态。解释可直接回到诸如“pitch falls then raises”“raises cheek”等文本片段。

## Baselines / Comparisons
比较 DNN-base、early fusion、两种 late fusion、BERT/RoBERTa 的 CLS/SEP/Mean pooling 与 freeze/fine-tune，任务是 self-reported 和 third-party sentiment。另用 ChatGPT 对 modality description 做直接预测，但不与判别模型做严格同过程对照。

## Experiments / Findings
在 self-reported sentiment 上，DNN-base 的 L+A/L+F/L+A+F 为 57.79/57.15/58.77，ours 的最好组合约 59.99/58.85/59.99；third-party 上 DNN-base 为 83.26/84.16/83.29，ours 最好约 85.62/85.71/85.86。L+A 往往比只用单模态好，说明文字描述既保留融合效果又显式暴露 modality evidence；ChatGPT 组合 A+F/L+A+F 也比单模态提升。

## Ablation / Error Analysis
audio/facial 描述质量决定收益；facial 动作直接写成“raises lip”不总是自然 sentiment 语言，组合 F 的结果可能下降，而 audio 提供更清晰 cues。separator 与 paragraph construction 结果不同，third-party sentiment 更偏好自然 paragraph；作者的 case study 显示模型可指出 language/audio/face 三组具体证据。

## Limitations
把连续信号转成文本会损失细粒度信息且依赖描述模板，LLM 读懂描述不等于原始 modality 真被使用。数据、评审和 sentiment label 规模有限，解释是输入级可读证据，不是模型内部因果分解。

## 两句话总结
这篇论文用自然语言描述替代不可读的 multimodal feature vector，让 sentiment 模型的融合证据可以被人直接检查。结果显示语言+音频/脸部描述能保持或小幅提升 F1，但描述质量和文本化过程也会引入新的偏差。

## Evidence note
已读取 modality description 构造、全部融合 baseline、self/third-party F1 表、ChatGPT 对照和 paragraph/description error analysis。
