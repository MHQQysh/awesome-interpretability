# 333. Uncertainty-Aware Explainable Recommendation with Large Language Models

- **Authors:** Yicui Peng, Hao Chen, Ching-Sheng Lin, Guo Huang, Jinrong Hu, Hui Guo, Bin Kong, Shu Hu, Xi Wu, Xin Wang
- **Venue / year:** IJCNN 2024
- **Paper:** https://arxiv.org/abs/2402.03366
- **Tags:** `explainable-recommendation` `prompt-learning` `GPT-2` `multi-task-learning` `uncertainty`

## Introduction
Recommendation systems commonly expose the item but not the reason for the recommendation. Textual explanations can increase user trust, yet fully fine-tuning an LLM for every recommender is expensive and user/item IDs do not carry useful meaning when inserted as ordinary words. The paper asks whether a recommender can learn continuous prompts that encode user-item identity and then use a frozen or lightly adapted GPT-2-style generator to produce personalized explanations.

The title's uncertainty component addresses a second problem: rating prediction and explanation generation are two losses with different scales and noise. A manually chosen fixed loss weight can cause one task to dominate. The proposed joint objective learns uncertainty weights so that the rating and generation tasks are balanced during training.

## Method / Framework
Each user ID and item ID is represented by a trainable vector rather than a textual token. The prompt sequence is `[user embedding, item embedding, explanation prefix]`. GPT-2 consumes the continuous user/item vectors followed by the explanation tokens and autoregressively predicts the next word. The ID embeddings are not intended to be human-readable; they are optimized as task prompts that capture collaborative information.

The recommender branch uses matrix factorization: the predicted rating is the inner product of the user and item vectors. The generator branch uses negative log likelihood for the observed explanation. These two losses are combined in a multi-task objective with learned homoscedastic uncertainty weights, so the model can adjust how strongly it prioritizes rating prediction versus explanation generation. At inference time, the user and item vectors are prepended and words are generated until EOS or a length limit.

The explanation metrics explicitly distinguish textual overlap from explainability. BLEU and ROUGE measure lexical quality; Unique Sentence Ratio (USR) measures whether explanations collapse to repeated templates; Feature Coverage Ratio (FCR) measures whether the text mentions relevant item features; Feature Diversity (DIV) measures diversity across generated explanations, where lower DIV is better in the paper's convention.

## Baselines / Comparisons
The four named baselines are Attn2Seq, NRT, PETER, and PEPLER. Attn2Seq uses an attribute/attention module to condition review generation. NRT jointly predicts ratings and generates summary-style explanations. PETER is a personalized Transformer for explainable recommendation. PEPLER is a prompt-learning method for personalized explanation generation. These baselines cover attribute-conditioned generation, joint rating/text learning, Transformer personalization, and prompt-based generation.

The data are Yelp, TripAdvisor, and Amazon reviews. The reported sizes are 27,147/20,266 users/items and 1,293,247 interactions for Yelp, 9,765/6,280 and 320,023 interactions for TripAdvisor, and 7,506/7,360 and 441,783 interactions for Amazon. The authors use five random splits and report the averaged results.

## Experiments / Findings
The method improves explanation-specific metrics on all three datasets. The headline values in the abstract are DIV 1.59 on Yelp, USR 0.57 on TripAdvisor, and FCR 0.41 on Amazon. In the detailed table, the method reaches Yelp USR 0.30 and FCR 0.34, TripAdvisor USR 0.57 and FCR 0.50, and Amazon USR 0.53 and FCR 0.41. The strongest gains are on TripAdvisor: USR rises from 0.02 for Attn2Seq to 0.57, and FCR rises from 0.02 for NRT to 0.50.

The model does not win every BLEU/ROUGE column. Its main advantage is that continuous ID prompts expose user-item-specific structure to GPT-2, producing more varied explanations that cover features. Textual quality remains comparable to the baselines, and the method is competitive or better on several ROUGE columns. This is a useful distinction: higher explainability is not obtained by simply copying a single high-overlap review sentence.

The dataset comparison also reveals a scale/generalization effect. TripAdvisor is the smallest of the three settings, and the paper reports some degradation in textual metrics there, but it still obtains the largest gains in USR/FCR. In other words, the approach improves the content diversity and feature coverage of explanations even when the amount of interaction data is limited.

## Ablation / Error Analysis
The joint-training design is the key implicit ablation: the rating task supplies collaborative signals to the user/item vectors while the language task forces those vectors to become useful prefixes for explanation generation. Without that shared objective, continuous prompts would have less direct pressure to encode why an item is recommended.

The uncertainty weighting is intended to prevent the rating loss from overwhelming the language loss or vice versa. The paper does not claim that each generated sentence is a faithful causal explanation of the recommender; FCR is feature coverage measured from the text, not a counterfactual test of whether removing the mentioned feature changes the recommendation score. This is the main interpretability caveat when comparing high FCR with true model faithfulness.

The baseline pattern is itself an error analysis. Attn2Seq and NRT often produce repetitive, low-coverage text; PETER and PEPLER are more competitive because they are personalized/prompt-based. The proposed model retains the prompt-learning advantage while using the rating branch and uncertainty balancing to improve coverage.

## Limitations
The user and item vectors are continuous and opaque, so the prompt is effective for generation but not directly interpretable to a human. The explanations are trained from review text and feature labels, which can contain noisy or incomplete reasons. BLEU, ROUGE, USR, FCR, and DIV do not establish causal faithfulness to the underlying recommender. The experiments are limited to three review domains and GPT-2; results may change for larger instruction-following LLMs, cold-start users/items, or recommendation settings without rich textual reviews.

## 两句话总结
这篇文章把 user/item ID 变成可学习的连续 prompt，并用 GPT-2 同时做评分预测和解释生成，再用 uncertainty weighting 平衡两个任务。它在 Yelp、TripAdvisor、Amazon 上主要提升了解释的多样性和特征覆盖，说明 prompt learning 能提升可读性，但文本指标本身还不能证明解释真正忠实于推荐决策。

## Evidence note
This note was written from the paper PDF (arXiv:2402.03366v1), including the continuous-prompt equations, uncertainty-weighted objective, dataset statistics, baseline definitions, and Table III results.
