# 254. LExT: Towards Evaluating Trustworthiness of Natural Language Explanations

- **Authors:** Krithi Shailya, Shreya Rajpal, Gokul S. Krishnan, Balaraman Ravindran
- **Venue / year:** ACM 2025; arXiv 2504.06227
- **Paper:** https://arxiv.org/abs/2504.06227
- **Tags:** Explanation Evaluation, Faithfulness, Plausibility, Healthcare, Trustworthiness

## Introduction

LLM 的自然语言解释可能流畅、像专家，却不一定忠实于输入或模型决策。BLEU/ROUGE 与普通 embedding similarity 主要衡量表面或语义接近，无法惩罚“医学术语很多但答非所问”的解释。LExT 的问题是如何把 plausibility、faithfulness、context relevance、consistency 等维度组合成面向高风险场景的 trustworthiness evaluation。

## Method / Framework

框架把解释质量拆成两类。Plausibility 关注解释是否符合专家知识、语义是否正确、是否与问题相关；faithfulness 关注解释是否能支持或预测模型的输出，而不是仅仅看起来合理。其 correctness 指标先计算 ground-truth 与生成解释的 BERT embedding cosine similarity，再用 MedNER/DeBERTaV3 抽取医学实体，按实体重叠加权，抑制空白或无关文本拿到虚高分。Context relevancy 在 NER 不足时让 Llama-70B 从生成解释反推问题，再与原问题计算相似度。Consistency 用多次迭代与 paraphrase 的 cosine variance 定义稳定性，最后整合为 Language Explanation Trustworthiness Score (LExT)。

## Baselines / Comparisons

实验比较六个模型，包含通用模型与医疗领域微调模型，并将传统 cosine/BERT 相似度与 NER-weighted accuracy、context relevancy 和 iterative/paraphrase stability 对照。作者还用 GPT-4 评估 faithfulness，并通过专家/人工评价检查自动指标是否忽略事实与上下文。重点不是宣称一个模型的绝对 SOTA，而是展示只看 embedding similarity 会把不完整、模板化甚至无关解释评得过高。

## Experiments / Findings

医学示例中，空白解释也能得到约 0.31 的 cosine similarity，半截文本可得到约 0.58，说明单一 embedding 分数会失真；包含大量医学实体但回答无关问题的文本也可能得到 0.753 accuracy，而反推问题的 context relevancy 只有 0.389。另一个迭代示例从 0.8668 的 coherent explanation 降到 0.3233 左右的无关文本，直接暴露了模型输出的不稳定。结论是解释可信度必须同时看“说得像不像”“是否支持输入/决策”“换次提示是否仍然一致”。

## Ablation / Error Analysis

去掉 NER 权重会让领域关键实体与空白/噪声文本混淆，去掉 context fallback 会漏掉“术语正确但问题不相关”的错误；只用 cosine 也无法识别迭代退化。NER 指标自身会受实体识别器、同义医学表述和预测解释中零实体影响；用 Llama-70B 反推问题又引入第二个模型的偏差。指标是 evaluation framework，不是对模型内部因果 faithfulness 的直接证明。

## Limitations

实验主要围绕医疗文本，ground truth explanation 的质量和领域专家分歧会影响分数；部分 faithfulness/context 判断依赖 GPT-4 或大模型。LExT 的多维分数尚未证明与真实临床决策收益、用户信任或安全事件强相关，跨语言、跨领域和低资源实体识别仍待验证。

## 两句话总结

LExT 将解释可信度从单一文本相似度扩展为实体加权正确性、上下文相关性、faithfulness 和多次生成稳定性的组合评估。它最重要的发现是流畅且含有专业词的解释可能仍然无关或不稳定，因此高风险部署需要多维、人工校验的解释评价。

## Evidence note

本笔记逐段核对 arXiv PDF 的 Introduction、metric definitions、Tables 1-4、实验与 Conclusion；示例数值来自正文。

