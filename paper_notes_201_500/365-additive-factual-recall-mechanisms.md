# 365. Summing Up The Facts: Additive Mechanisms Behind Factual Recall in LLMs

- **Authors:** Bilal Chughtai, Alan Cooney, Neel Nanda
- **Venue / year:** NeurIPS 2023 Attributing Model Behaviour at Scale Workshop
- **Paper:** https://arxiv.org/abs/2402.07321
- **Tags:** `mechanistic-interpretability` `factual-recall` `direct-logit-attribution` `attention-heads`

## Introduction
The paper studies the basic factual-recall prompt `Fact: The Colosseum is in the country of` and asks how a Transformer retrieves the correct attribute. Prior work localized memories or analyzed narrow circuits, but could miss the fact that several independent sources and mechanisms contribute to the answer.

The proposed “additive motif” says that multiple qualitatively different components can each boost the correct logit, even when no component alone makes the answer the argmax. Their constructive interference makes the complete model robust, and this motivates analyzing subject and relation information together.

## Method / Framework
The authors build a hand-written dataset of 106 factual prompts across 10 relations and primarily study Pythia-2.8B, with additional GPT2-XL, GPT-J, and other Pythia sizes. They group attributes into subject-related and relation-related counterfactual sets and filter to facts the model knows, usually top-10.

Direct Logit Attribution (DLA) maps component outputs into vocabulary logits. The paper extends DLA by decomposing each attention head's output by source token, allowing separate measurement of Subject and Relation contributions. Heads are classified as Subject, Relation, or Mixed using source-specific DLA ratios; summed MLP effects are also analyzed.

## Baselines / Comparisons
The baseline is the conventional narrow circuit view that identifies a small set of heads or MLP layers without decomposing source tokens. The paper also compares component-level logit contributions, all-components residual output, and patching/intervention of candidate heads.

Mechanism comparisons are: Subject heads, which read enriched subject representations; Relation heads, which read the relation token and boost a category of relation attributes; Mixed heads, which combine both source positions; and MLPs, which broadly boost relation-associated attributes. The correct answer is compared with other subject and relation attributes, not only with a global accuracy number.

## Experiments / Findings
Subject heads attend mainly to subject tokens and extract subject attributes across relations; relation heads attend mainly to relation tokens and boost many attributes in the relation set, often independently of the subject. Mixed heads make two source-specific additive updates, while MLPs broadly boost relation attributes but do not preferentially select the correct one.

The correct attribute benefits from constructive interference among all four mechanisms. Patching the top relation heads between subjects does not substantially reduce performance, supporting the claim that those heads do not causally depend on the subject. Similar mechanisms appear in other model families, although the boundaries are less clean in larger models.

The paper also gives a partial mechanistic account of the reversal curse: subject enrichment can support “A is B” recall without automatically producing the reversed “B is A.” The broader lesson is that a circuit explanation must include all information sources relevant to the final token, not only the most obvious subject-to-answer path.

## Ablation / Error Analysis
Head classification and source-token DLA are the key ablations. If DLA is aggregated over a full head, the two contributions of a Mixed head are hidden; source decomposition reveals that one head can read subject and relation evidence simultaneously. Patching candidate relation heads and comparing random or alternative heads tests whether the apparent relation mechanism is causally meaningful.

The authors warn that head boundaries are fuzzy, attributes are often polysemantic, and common high-frequency words dominate logit norms. MLP analysis is intentionally partial: summed MLPs boost many relation attributes, but the individual neurons and their composition with relation heads remain unresolved.

## Limitations
Most experiments use one main model and a small, manually curated dataset. The head taxonomy is useful but somewhat arbitrary, and it does not quantify the exact importance of every mechanism. The analysis focuses on common categorical facts and simple one-step prompts, not multi-hop recall, paraphrases, or arbitrary world knowledge.

## 两句话总结
这篇机制解释论文发现事实回忆不是由一个“记忆头”单独完成，而是由 subject heads、relation heads、mixed heads 和 MLP 的多路 logit 更新相加并对正确答案产生建设性干涉。它用按源 token 拆分的 DLA 证明窄电路分析会漏掉重要信息，但结论主要来自 Pythia-2.8B 和 106 条人工事实，不能直接推广到所有知识行为。

## Evidence note
本笔记依据 arXiv v1 全文逐节核对；数据集规模、模型和四类机制均来自正文与附录限制说明。
