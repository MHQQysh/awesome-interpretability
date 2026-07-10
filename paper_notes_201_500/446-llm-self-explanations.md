# 446. Can Large Language Models Explain Themselves? A Study of LLM-Generated Self-Explanations

- **Authors:** Shiyuan Huang, Siddarth Mamidanna, Shreedhar Jangam, Yilun Zhou, Leilani H. Gilpin
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2310.11207
- **Tags:** `self-explanation` `feature-attribution` `faithfulness` `ChatGPT` `LIME`

## Introduction
ChatGPT 会主动解释情感判断或数学答案，但解释是否真的支持 prediction，还是只是在生成一个听起来像人类理由的 post-hoc rationale，并不清楚。本文在 SST sentiment classification 上把 ChatGPT 自解释与 occlusion/LIME 放在同一套 faithfulness 和 disagreement 指标下比较，重点区分 explain-then-predict（E-P）与 predict-and-explain（P-E）。

## Method / Framework
模型被要求输出每个词的 attribution score 或只输出 top-k 重要词；E-P 先生成解释再做 sentiment prediction，P-E 先预测再生成解释。由于 ChatGPT 无 gradient，传统对照采用 word occlusion 和 LIME；评价指标包括 comprehensiveness、sufficiency、decision flip by most important token、需要删除多少 token 才翻转以及 deletion rank correlation。另用 explanation agreement/rank correlation 比较不同方法是否选中相似词。

实验从 SST test 随机抽 100 句，temperature=0；E-P/P-E 的系统 prompt、score format 和 top-k prompt 分开处理。作者特别提醒不同 system prompt 会让同一个 ChatGPT API 表现为不同“模型”，所以解释之间不是简单把同一模型的两种输出相减。

## Baselines / Comparisons
对照为 prediction-only ChatGPT、E-P/P-E full self-explanation、E-P/P-E top-k self-explanation、occlusion 和 LIME。评价同时看准确率与 faithfulness，避免只因解释看起来合理就取胜；LIME 的成本也被记录，因为默认实现需要约 5,000 perturbations，论文自适应后平均仍约 183 次、每条解释约 20 分钟。

## Experiments / Findings
prediction-only accuracy 为 92%，E-P 为 85%，P-E 为 88%，E-P top-k/P-E top-k 分别为 80%/83%，说明让模型先编写 attribution 可能损害情感分类。faithfulness 上没有绝对赢家：E-P 的 self-exp comp/suff/DFMIT/DFFrac/RankDel 为 0.19/0.25/0.16/0.55/-0.03，LIME 为 0.17/0.22/0.13/0.50/-0.02；P-E 的 self-exp 与 LIME 也非常接近。

self-explanations 与传统方法在 agreement 指标上高度不一致，但 faithfulness 分数相近，提示“不同解释选了不同词”不等于其中一个一定错误。自解释的优势是生成预测时几乎没有额外解释器成本，可能成为 LIME 的低成本替代；其缺点是 score 常是 0.25/0.5/0.75 等过度规整的离散值，不像真实细粒度影响。

## Ablation / Error Analysis
full score 与 top-k 的对比显示 top-k 并不稳定地更好；在 top-k adapted metrics 中，E-P self-exp comp@k/suff@k/DFMIT@k 为 0.11/0.32/0.32，LIME 为 0.17/0.28/0.40，仍无统一优胜者。单词删除对 ChatGPT 预测经常几乎没有影响，导致 RankDel、DFMIT 受随机 tie-breaking 和模型可从上下文恢复缺词的行为影响。

低分可能来自 explanation 不忠实，也可能来自评测本身不适合人类式 LLM：人类不会给“删除一个词使概率改变多少”这样的精确数值。作者还发现 zero-shot/少量 prompting 不能解决 rounded score，且不同 explanation 方法的 agreement 高度不稳定。

## Limitations
只研究 ChatGPT、SST 和 100 条短文本，无法覆盖数学 CoT、开放域知识或更强模型；无白盒 gradient、causal ground truth 和重新训练的可控任务。faithfulness 指标依赖 model textual confidence，传统删除方法的自然性和 top-k 适配也会改变结论；论文明确说更好的解释可能被当前评测区分不出来。

## 两句话总结
本文发现 ChatGPT 的自解释在 faithfulness 指标上大致不差于 occlusion/LIME，却与它们选择的特征高度不同，说明“解释一致性”和“解释忠实”是两个问题。更值得警惕的是，生成解释会把预测准确率从 92% 降到 85-88%，而 rounded scores、删除不敏感性和评测 ill-posedness 使传统解释排行榜难以直接适用于 LLM。

## Evidence note
已逐段读取本地 PDF，核对了 E-P/P-E/full/top-k 设计、SST 100 句、92/85/88/80/83 accuracy、Table VII/VIII faithfulness、LIME 成本和 model-sameness/rounded-score 分析。
