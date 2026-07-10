# 111. Understanding Faithfulness and Reasoning of Large Language Models on Plain Biomedical Summaries

> 人工精读笔记：Findings of EMNLP 2024。重点是 biomedical plain-language summary 的专家 faithfulness/ reasoning 标注，以及通用 metric 的域迁移失败。

## 0. 论文信息

- **作者**：Biao Zhang 等
- **来源**：[Findings of EMNLP 2024](https://aclanthology.org/2024.findings-emnlp.578)
- **数据**：175 篇 biomedical articles、1445 个摘要句子；7 个 LLM 生成摘要
- **指标**：句子 faithfulness、abstractiveness、readability、supporting sentence retrieval

## 1. Introduction：要解决什么问题

医学摘要既要让公众读懂，又不能改变研究发现、方法和结论。通用新闻摘要 faithfulness metric 可能不了解医学术语、证据链和隐含限定，因此需要专门的医生标注与 reasoning 级分析。

## 2. Method / Framework：怎么解决

作者让医学标注者判断每个 summary sentence 是否被 source article 支持，并高亮 supporting evidence、给出 rationale；再比较 GPT-4、Claude、Gemini、Llama 等模型生成的 plain summaries。另按 abstractiveness 高低分组，测试摘要越“改写”是否越容易失真。

## 3. Baseline / 对比基线

通用 QuESTEval、QAFactEval、SummaC、BLEURT 等自动 faithfulness metric；LLM-based evaluator；Okapi BM25 supporting-sentence retrieval；只给 binary label 与要求 evaluator 生成 reasoning 的 prompt。

## 4. Experiments / Findings：结果如何读

更高 abstractiveness 与 non-faithful factual hallucination 正相关；更易读的摘要也可能加入未被原文支持的细节。七个模型在 faithfulness、readability 和 abstractiveness 间存在不同 trade-off，不能只按语言质量排序。

通用 metric 在 biomedical domain 的 agreement 较差，直接迁移新闻摘要 evaluator 会漏掉医学限定和证据错误。要求 evaluator 同时返回 supporting sentences/reasoning 通常比只返回 label 更有帮助，但仍不能完全解决域偏差。BM25 在 low-abstractiveness 摘要上较强，高抽象改写时检索支持句明显变难。

## 5. Ablation / 机制解释

abstractiveness 高低分组、LLM-only vs metric、binary label vs label+evidence、BM25 top-k 和 evaluator prompt 都被对照；结果说明 faithfulness 评估难点来自医学领域知识和改写程度，而不只是模型大小。

## 6. Limitations / 局限与复现注意

专家标注 1445 句约需 110 人时、成本约 5500 美元，难以大规模扩展；医学文本类型、语言和文章领域有限；supporting sentence 标注仍存在医生间分歧，不能把自动 metric 的低 agreement 只归咎于模型。

## 7. 两句话总结

论文构建了 plain biomedical summary 的细粒度专家 faithfulness/reasoning 数据，发现更高抽象性和更高可读性都可能伴随事实不忠实。通用摘要指标在医学域迁移效果差，要求 evaluator 给证据和推理能改善判断，但专家标注成本和领域特异性仍是主要瓶颈。
