# 155. CHiLL: Zero-shot Custom Interpretable Feature Extraction from Clinical Notes with Large Language Models

- **Authors:** Denis Jered McInerney, Geoffrey Young, Jan-Willem van de Meent, Byron C. Wallace
- **Venue:** EMNLP Findings 2023
- **Paper:** https://aclanthology.org/2023.findings-emnlp.568
- **Tags:** Clinical NLP, Interpretable Features, Zero-shot, Linear Model

## Introduction

临床风险预测常用线性模型和手工 feature，因为医生需要看到 feature weight；直接把 EHR 送入黑盒 LLM 会牺牲可审计性，手工从长 clinical note 抽取高层概念又昂贵。论文问：领域专家能否用自然语言定义“慢性病”“既往住院”等 feature，再让 LLM zero-shot 从 note 中抽取这些 feature，最后由简单线性模型完成预测？

## Method / Framework

CHiLL（Crafting High-Level Latents）让专家为每个 feature 写自然语言 query。Flan-T5 对每条 clinical note 回答 query，得到二值或多值 feature；这些 noisy labels 作为输入训练 logistic regression。最终预测可分解为 feature activation 加线性权重，医生可以检查 feature 定义、患者证据和权重方向。

实验使用 MIMIC-III、MIMIC-CXR 以及 30-day readmission 等任务，并比较零样本抽取 feature 与 reference features、Bag-of-Words、直接文本模型的效果和可解释性。

## Baselines / Comparisons

比较 BoW linear model、专家或数据集提供的 reference features、低级词频特征、LLM 自动 feature、以及端到端 neural / clinical model。指标包括 AUROC / AUPRC、accuracy 或 F1，并由临床背景评审检查 feature weights 是否符合预期。

## Experiments / Findings

- CHiLL 线性模型的预测性能与 reference-feature linear model 相当，说明高层 noisy feature 即使不人工标注，也能保留实用信号。
- 相比 BoW，CHiLL 的 feature 名称和权重更接近临床概念，医生可直接解释“哪些病史导致风险提高”。
- Flan-T5 等可本地运行模型能够完成零样本 feature extraction，减少把敏感 EHR 发给闭源 API 的需要。
- learned feature weights 大体符合临床预期，但某些 query 会被模型用词面相关线索近似回答，导致 feature precision 下降。

## Ablation / Error Analysis

消融不同 feature set、query wording、Flan-T5 规模、linear / BoW 输入和数据集。高层 feature 过于宽泛会混入多个病症，过于具体又稀疏；LLM 抽取误差会作为 label noise 进入线性模型。feature weight 的方向不能单独证明临床因果，且 note 中否定、时间和共指是常见错误来源。

## Limitations

MIMIC 数据和任务只是 proxy，不能代表所有医院和人群；专家 query 设计仍需医学知识。零样本 LLM 可能漏读长 note、误解否定或产生偏差，线性权重也只解释预测器而不解释 LLM 抽取过程。临床部署需要隐私、校准、外部验证和医生监督，不能把相当的 AUROC 当成安全等价。

## 两句话总结

CHiLL 把专家自然语言 feature 定义交给 LLM zero-shot 抽取，再用线性模型完成可审计的临床预测。它在 MIMIC 任务上接近 reference-feature 模型并明显比 BoW 更易解释，但抽取噪声、医学语境和外部验证仍决定能否真正使用。
