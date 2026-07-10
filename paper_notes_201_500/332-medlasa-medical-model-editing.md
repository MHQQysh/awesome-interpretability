# 332. Editing Factual Knowledge and Explanatory Ability of Medical Large Language Models

- **Authors:** Derong Xu, Ziheng Zhang, Zhihong Zhu, Zhenxi Lin, Qidong Liu, Xian Wu, Tong Xu, Wanyu Wang, Yuyang Ye, Xiangyu Zhao, Enhong Chen, Yefeng Zheng
- **Venue / year:** CIKM 2024
- **Paper:** https://arxiv.org/abs/2402.18099
- **Tags:** `model-editing` `medical-llm` `causal-tracing` `explanations` `locality`

## Introduction
Medical LLMs need both correct factual knowledge and the ability to explain complex medical facts. Existing model-editing work is mostly evaluated on general encyclopedic facts and usually asks whether a single answer changed; it does not adequately test whether a medical edit generalizes to paraphrases, preserves unrelated medical knowledge, or changes a multi-sentence explanation in a controlled way. The paper frames two related problems: editing a medical fact, and editing the explanatory behavior associated with a fact.

The authors argue that medical knowledge is difficult for locate-then-edit methods because it can be distributed across multiple layers and because an explanation requires a chain of related concepts, not just a head-relation-tail triple. A method that edits too broadly may fix the target fact but damage fluency or neighboring facts; a method that edits one layer may fail to change the relevant explanation. This motivates a layer-aware, scalable adapter rather than a one-shot weight overwrite.

## Method / Framework
MedLaSA is a Layer-wise Scalable Adapter strategy. First, causal tracing is used to measure how strongly different layers and parameter groups are associated with the target knowledge. The association scores are normalized into a layer-wise scale set. MedLaSA then inserts trainable adapters into dense attention/MLP weights, with both adapter rank and adapter weight scaled according to the traced association for that particular edit.

The important design choice is per-knowledge and per-layer scaling. A hard, fixed adapter applied to every layer treats all edits as equally distributed. MedLaSA instead gives larger rank or weight to layers that causal tracing identifies as important, and smaller intervention to layers with weak association. Semantically similar inputs receive consistent scales, while unrelated inputs should activate a different scale profile. This combines the extra-parameter capacity of adapter editing with the localization principle of ROME/MEMIT-like methods.

The paper introduces two benchmarks. MedCF is a Medical CounterFact-style set for editing factual knowledge, with original facts, counterfactual targets, rephrased queries, and unrelated queries. MedFE is built from MedMCQA and targets explanatory ability: the edit target includes a factual statement and an expert explanation, while efficacy and generality test whether the model produces the revised explanation for the original and rephrased questions. The evaluation separates Efficacy, Generality, Locality, and Fluency. Locality is further decomposed into target-distribution, entity-mapping, structural-similarity, textual-similarity, and consistent-topic checks.

## Baselines / Comparisons
The baselines are multi-layer fine-tuning (FT), LoRA, ROME, MEND, and MEMIT, implemented through EasyEdit with shared experimental settings. The edited backbones are ChatDoctor-7B and Meditron-7B, and the experiments focus on single-edit cases. FT tests the destructive upper bound of changing many original parameters; LoRA tests a generic low-rank adapter; ROME tests single-layer locate-then-edit; MEMIT tests mass/multi-edit style parameter updates; MEND tests a learned hypernetwork editor.

The authors also compare MedLaSA's causal-tracing layer selection with Random and Fixed strategies. Random chooses layer scales without localization, while Fixed applies the same scale across layers and examples. These are more informative than only comparing against named methods because they isolate whether the layer-wise scale, not merely the presence of an adapter, drives the result.

## Experiments / Findings
MedLaSA improves the overall efficacy/generality-locality trade-off on both MedCF and MedFE and on both medical backbones. In the reported MedFE row for ChatDoctor-like settings, MedLaSA reaches 98.11 Efficacy, 93.58 Generality, 89.25 and 84.11 on the main locality submetrics, with a high aggregate score; the second backbone shows a similar pattern, with 98.77 Efficacy and 94.81 Generality. The exact submetric names should be read with the table header because the PDF lays several locality and fluency columns side by side, but the consistent result is higher preservation and fluency rather than only a higher edit success rate.

The baseline behavior explains the advantage. FT can cause model collapse or a marked fluency loss because too many original weights are changed. LoRA adds capacity but does not localize which knowledge or layers should be affected, so its locality is weak. ROME is strong on counterfactual triples but assumes a single-layer association and is less suitable for long explanations. MEMIT is effective on MedCF efficacy/generality but can alter unrelated knowledge. MEND is sensitive to initialization and has lower average performance on these medical datasets.

The paper's strongest interpretability result is that causal tracing is not only a diagnostic visualization. Its association matrix is used to determine where and how much the adapter should edit. The causal-tracing plots show that target and rephrased inputs share relevant layers, while unrelated knowledge has a different layer profile. This is the mechanism by which MedLaSA aims to edit semantically identical knowledge without spreading the update to neighboring facts.

## Ablation / Error Analysis
Removing Scaling Rank (SR) decreases all metrics, especially locality, showing that rank controls the capacity needed to represent the edited medical relation. Removing Scaling Alpha (SA) can improve generality but causes clear locality losses: on MedFE, the reported CT locality falls from 89.25% to 82.16% and TS from 84.11% to 75.84%. Removing both components produces the largest efficacy and generality degradation.

The editable-weight ablation compares attention-only, MLP-only, and combined updates. MLP weights usually give higher edit success, but updating more MLP and attention weights at once can reduce locality and fluency. The result is a real trade-off rather than “more editable parameters is always better”: MedLaSA uses traced scales to reserve capacity for the relevant layers and keep unrelated layers quiet.

The rank/alpha sweeps also show that larger adapter capacity can improve efficacy and generality while harming locality. This is why the paper reports weighted aggregate scores instead of selecting the method by target accuracy alone.

## Limitations
The study focuses on single-edit problems and two medical LLM backbones, so it does not establish that MedLaSA remains stable under long sequences of edits or under conflicting updates. MedCF and MedFE are newly constructed benchmarks and therefore may not cover the full distribution of clinical language, multilingual medicine, or real patient conversations. Causal tracing itself is an approximation and can miss compensation between components. Finally, high locality on synthetic QA probes does not guarantee clinical safety; the method should not be interpreted as a substitute for medical validation.

## 两句话总结
MedLaSA 用 causal tracing 找到每条医学知识在不同层的关联强度，再用 layer-wise scalable adapters 按知识分配 rank 和权重，从而同时支持事实编辑和复杂解释编辑。相较 FT、LoRA、ROME、MEND、MEMIT，它在 MedCF/MedFE 上取得更好的编辑成功率与 locality/fluency 平衡，但当前证据主要覆盖单次编辑和合成医学基准。

## Evidence note
This note was written from the CIKM PDF (arXiv:2402.18099), including MedCF/MedFE construction, causal-tracing equations, baseline definitions, Table 4 main results, Table 5 layer-selection comparison, Table 6 ablation, and the limitations section.
