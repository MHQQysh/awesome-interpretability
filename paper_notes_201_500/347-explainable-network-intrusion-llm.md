# 347. Towards Explainable Network Intrusion Detection using Large Language Models

- **Authors:** Paul R. B. Houssel, Priyanka Singh, Siamak Layeghy, Marius Portmann
- **Venue / year:** BDCAT 2024
- **Paper:** https://arxiv.org/abs/2408.04342
- **Tags:** `network-intrusion-detection` `NetFlow` `explainability` `GPT-4` `Llama-3`

## Introduction
Academic NIDS papers often report near-perfect scores on synthetic or skewed benchmarks, while real network traffic is harder and security analysts still need to understand an alert. This paper tests whether a general LLM can directly classify malicious NetFlows and provide a natural-language explanation, rather than replacing the language-generation head with a conventional classifier.

The paper is exploratory and deliberately asks whether LLM pretraining is useful without relying on artificially balanced, domain-specific training data. It compares the detection capability with lightweight traditional models and then argues that LLMs may be more useful as explanation/response agents attached to a conventional NIDS.

## Method / Framework
NetFlow rows are serialized as key-value text such as `L4_DST_PORT: 80`. GPT-4-0613 and Llama-3-8B-Instruct receive an instruction to output `1` for malicious and `0` for benign. The evaluation uses NF-UNSW-NB15-v2 and NF-CSE-CIC-IDS2018-v2, split 95/5 with stratification and evaluated by 10-fold cross-validation on a small test portion because LLM inference is expensive.

Llama-3 is also fine-tuned with KTO and ORPO. KTO uses relevant/irrelevant response labels; ORPO uses accepted true labels and rejected inverse labels. Explanations are manually inspected for true positives, false positives, true negatives, and false negatives rather than assigned a quantitative faithfulness score. The paper discusses adding RAG over cyber-threat intelligence and function calling as a future complementary architecture.

## Baselines / Comparisons
The primary comparison is GPT-4 versus Llama-3, zero-shot versus KTO/ORPO fine-tuning. External baselines include Random Forest, LSTM, Decision Tree, and FlowTransformer-style network models. The paper stresses that these traditional models are trained for NetFlow classification, whereas the LLMs use general pretraining and text serialization.

## Experiments / Findings
On NF-UNSW-NB15-v2, zero-shot Llama-3 reaches macro precision 48.56% and recall 40.56%; GPT-4 reaches 50.85% and 53.47%. On NF-CSE-CIC-IDS2018-v2, Llama-3 reaches 49.66%/49.73% and GPT-4 50.25%/51.02%. Fine-tuning produces only small changes: on UNSW, KTO with 10K samples reaches 53.98% precision and 50.36% recall, while ORPO with 10K reaches 49.25%/42.40% and with 50K 52.00%/43.50%; on CIC-IDS, KTO 10K reaches 52.22%/55.13% and ORPO 10K 50.89%/51.01%.

These numbers are close to chance and far below the cited specialized baselines, such as Random Forest F1 92.17% and LSTM F1 92.82% on NF-UNSW-NB15-v2. The paper's negative result is decisive: general LLMs are not suitable as standalone malicious-NetFlow detectors in this setup.

The qualitative explanation analysis is more promising. Llama-3 can point to ports, packet counts, protocol patterns, and other features and can explain simple attacks such as a SYN flood. However, it sometimes invents logic, fails to understand feature interactions, or gives the same generic rationale for different flows. The authors propose using it after a lightweight detector flags an alert, with RAG grounding the explanation in current CTI.

## Ablation / Error Analysis
The fine-tuning ablation shows that more labeled examples and preference optimization do not automatically solve the domain mismatch. KTO is usually slightly better than ORPO, but improvements are small. This suggests the bottleneck is not simply instruction following; it is learning the semantics and distributions of NetFlow features.

The efficiency comparison is stark. Llama-3-8B takes about 14,000 microseconds per inference, versus about 2.03 for Random Forest and 25.02 for the cited LSTM setup. The paper describes roughly 7,000-fold slower inference than lightweight models, making real-time standalone deployment impractical. Temperature, serialization, and prompt wording are also acknowledged sources of variance.

## Limitations
The evaluation is small because of API/GPU cost and uses two standardized datasets rather than live traffic. Explanation quality is qualitative and lacks human scoring or faithfulness intervention. GPT-4 cost and closed access limit reproducibility. NetFlow serialization may discard temporal/session context, and success on a text prompt would not guarantee safe threat response.

## 两句话总结
这篇论文把 NetFlow 表格序列化成文本，比较 GPT-4/Llama-3 与 Random Forest、LSTM 等 NIDS，发现通用 LLM 在恶意流量检测上接近随机且推理慢约数千倍。它真正有价值的方向是把 LLM 放在传统 NIDS 后面，用 RAG 和 function calling 生成有上下文的告警解释与响应建议，而不是让 LLM 直接承担检测。

## Evidence note
This note was written from the BDCAT paper PDF (arXiv:2408.04342v1), including the two dataset tables, KTO/ORPO results, external model comparison, qualitative explanation analysis, and inference-complexity table.
