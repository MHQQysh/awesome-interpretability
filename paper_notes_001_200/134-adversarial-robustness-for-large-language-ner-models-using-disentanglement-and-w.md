# 134. Adversarial Robustness for Large Language NER Models using Disentanglement and Word Attributions

- **Authors:** Xiaomeng Jin, Bhanukiran Vinzamuri, Sriram Venkatapathy, Heng Ji, Pradeep Natarajan
- **Venue:** EMNLP Findings 2023
- **Paper:** https://aclanthology.org/2023.findings-emnlp.830
- **Tags:** Adversarial Robustness, NER, Disentanglement, Integrated Gradients

## Introduction

LLM 在复杂 NER 上并不稳定，轻微词级修改也可能造成严重性能下降；已有攻击通常偏向替换非实体词，搜索空间有限，而且不少攻击样本虽然让标签翻转，却与原句语义不相似。作者要同时解决“攻击有效”和“语义保持”两个目标，并进一步用生成的攻击样本做 adversarial training，提高 NER 模型的鲁棒性。

论文的关键解释性假设是，NER 模型的表示混合了 entity influence 和 context / non-entity influence。若先把两类因素解耦，再用 word attribution 定位重要词，就能从实体和上下文两个空间更有针对性地生成攻击。

## Method / Framework

1. **Disentanglement。** 编码器得到输入表示后，训练 representation 分离器，使 entity 与 context 成分分别可预测；目标是让后续 attribution 不只偏向非实体词。
2. **Word attribution。** 使用 Integrated Gradients（IG）计算每个词对 NER 输出的贡献，选择 top-K entity words 和 context words。
3. **Entity substitution。** 用 Wikidata entity linking 找到同一实体类别的替换项，例如把 Manchester United 换成 Barcelona、Real Madrid 等同类实体。
4. **Context substitution。** 用 GPT-J 的 infilling language model 在 mask 位置生成 5 个候选词。
5. **Sentence ranking。** 把候选句与原句送入 Universal Sentence Encoder，按 cosine similarity 保留语义最接近且能诱发标签翻转的样本。

作者还把实体替换与上下文替换组合，并限制最多修改 1 个或 3 个词，最后用攻击样本训练 BERT NER、T5 instruction NER 和 LLaMA-2-7B NER。

## Baselines / Comparisons

攻击 baseline 包括 RockNER（Wikidata 实体替换加随机 mask 上下文）、SeqAttack 框架中的 Bert-Attack、CLARE、DeepWordBug。比较指标是 attack rate、modification rate、文本语义相似度和攻击后的 F1。防御部分在 CoNLL-2003、OntoNotes 5.0、英文 MultiCoNER 上比较 BERT、T5-Large instruction NER 与 LLaMA-2-7B，并报告原始模型与 adversarial training 后的 F1。

## Experiments / Findings

- 在 CoNLL-2003 上，约 22% 的修改率下，方法比 Bert-Attack 高约 10% attack success；相较 DeepWordBug-II，攻击后的 F1 低 8%，attack rate 高 24%。
- 论文摘要报告，使用生成攻击样本训练后，CoNLL-2003 的 F1 比原始 LLM NER 提高约 8%，OntoNotes 5.0 提高约 18%。在 instruction-fine-tuned T5 的 MultiCoNER 上，攻击可使 F1 下降约 10%，说明 instruction following 并不等于鲁棒。
- 实体和上下文都参与攻击。只改非实体词会漏掉模型对实体表面形式的依赖；只改实体又无法测试上下文偏差。
- 在不同数据集上，语义相似度 guardrail 使攻击不再只是 typo 或无关句改写；攻击样本更接近原句语义但仍能翻转 NER 标签。

## Ablation / Error Analysis

IG 与 Kernel-SHAP、attention、weighted linear regression 比较时，IG 在非线性 NER 模型上取得最强攻击效果。去掉 disentanglement 后，重要词选择更偏向单一词类，attack rate 与语义保持同时变差；去掉 USE ranking 会产生更多不自然样本。作者也分别比较实体级、上下文级、实体加上下文的修改预算，发现组合策略更能覆盖模型的决策脆弱点。

## Limitations

研究主要覆盖 encoder NER、T5 和 LLaMA-2-7B，没有完整探索 decoder-only LLM 的规模效应。方法假设白盒访问模型梯度或表示，并依赖 Wikidata、GPT-J 和 USE，部署成本高于纯黑盒攻击。攻击样本的语义相似度是 embedding 近似，不等于人工判断；此外，实体替换可能改变真实世界事实，应用到生产文本前需要额外安全约束。

## 两句话总结

论文把 disentangled representation、Integrated Gradients 和语义相似度结合起来，从实体与上下文两条路径生成更有效且更自然的 NER 对抗样本。实验显示该攻击比 RockNER、SeqAttack 家族更强，并能通过 adversarial training 显著提高多个 NER 模型的 F1，但白盒依赖和模型覆盖范围仍限制了结论。
