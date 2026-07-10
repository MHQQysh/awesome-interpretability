# 343. Large Language Models for Forecasting and Anomaly Detection: A Systematic Literature Review

- **Authors:** Jing Su, Chufeng Jiang, Xin Jin, Yuxin Qiao, Tingsong Xiao, Hongda Ma, Rong Wei, Zhi Jing, et al.
- **Venue / year:** arXiv 2024
- **Paper:** https://arxiv.org/abs/2402.10350
- **Tags:** `survey` `time-series` `forecasting` `anomaly-detection` `evaluation`

## Introduction
Forecasting and anomaly detection are dominated by numerical/time-series methods, while LLMs were originally trained for language. The review asks what it actually means to use an LLM in these settings: directly prompt a general model with numbers, convert a time series into text, pretrain a foundation model on time series, fine-tune a language model, or use the LLM only for semantic context and explanation.

The authors emphasize that the attraction is not simply accuracy. LLMs may unify numerical signals with news, logs, descriptions, and domain knowledge, and may produce explanations. But time series contain seasonality, missing values, non-stationarity, rare anomaly labels, and distribution shifts that are not solved by natural-language fluency.

## Method / Framework
This is a systematic literature review rather than a new forecasting model. The review defines three guiding questions: which methodologies are used for forecasting across domains; how LLM anomaly detection compares with traditional methods; and what limitations/challenges remain. It searches multiple engines and databases, including OpenReview, and applies explicit inclusion/exclusion criteria requiring a claimed LLM use in forecasting or anomaly detection.

The paper groups the literature by task, model, training strategy, dataset, and metric. Table 3 records representative systems using GPT-2/3/4, Llama-2, BERT, RoBERTa, XLNet, BART, BigBird, Pegasus, and Transformer variants across ETT, Weather, Darts, Monash, ICEWS, METR-LA, PeMS, KPI, HDFS, BGL, Yahoo, SMD, OpenStack, and other datasets.

## Taxonomy / Scope
The review distinguishes two forecasting routes. The first converts a time series into a representation consumable by a general LLM, through prompting, serialization, tokenization, or textual context. The second trains or adapts a foundation model specifically on time-series data, then transfers it to downstream forecasting. For anomaly detection, the review separates foundation-model feature extraction, prompt-based classification, few-shot use, and domain fine-tuning.

It also maps evaluation practices: forecasting uses MAE, MSE, RMSE, MAPE, and sMAPE; anomaly detection uses precision, recall, F1, AUROC, and accuracy. The survey discusses data preprocessing, imputation, normalization, windowing, label scarcity, and the interaction between numerical inputs and unstructured text such as news or logs.

## Baselines / Comparisons
The comparison baseline is the conventional time-series ecosystem: statistical models, classical machine learning, RNN/GRU/LSTM systems, Transformer forecasters, and specialized anomaly detectors. The review does not claim that every LLM paper beats these baselines. Instead, it compares whether the LLM is used as a predictor, feature extractor, prompt-conditioned reasoner, or semantic assistant, and asks whether each use is justified by the data and metrics.

The table-level comparison reveals that many papers use BERT-like models for anomaly detection and GPT/Llama-like models for forecasting, but metrics and datasets differ substantially. Therefore cross-paper “state of the art” claims are not directly portable.

## Experiments / Findings
The review's main synthesis is that LLMs are most promising when the task contains heterogeneous context. News, logs, metadata, and domain descriptions can be represented in language, allowing an LLM to combine textual cues with temporal signals. Direct zero-shot numerical forecasting is less reliable, especially when the model has not been trained on the relevant scale, sampling frequency, or seasonality.

For anomaly detection, language models can help with log semantics and cross-domain transfer, but rare anomalies and false-positive costs remain difficult. Foundation models offer reusable representations, while prompt/few-shot methods reduce labeled-data requirements. The resulting gains are often offset by computation, latency, and the need to calibrate thresholds for each deployment.

The review identifies recurring data problems: missing values, noisy/unstructured text, complex seasonal patterns, nonlinearity, and insufficient anomaly labels. It also notes that LLM hallucination is especially dangerous when the system must explain an anomaly or forecast without evidence. The strongest future direction is hybrid modeling: numerical encoders and specialized time-series modules provide quantitative fidelity, while LLMs provide semantic context, reasoning interfaces, and explanations.

## Ablation / Error Analysis
Because this is a review, there is no shared ablation table. Its error analysis is methodological: papers that compare an LLM against a baseline trained on different data, different windows, or different preprocessing are not strictly like-for-like. Accuracy can also hide class imbalance in anomaly detection, while a text explanation can be persuasive but unsupported.

The survey stresses that prompt strategy, tokenization, and input serialization are part of the model. Changing the numerical-to-text representation can change performance dramatically, so “LLM for time series” is not one reproducible method. It also warns that the absence of labels makes future-event evaluation and anomaly evaluation difficult, especially under distribution shift.

## Limitations
The field is fast-moving and the reviewed papers use heterogeneous definitions of LLM, datasets, metrics, and task splits. Some included studies are preliminary or rely on synthetic benchmarks. The review cannot establish a universal ranking, and its conclusions may age quickly as time-series foundation models improve. Interpretability is also under-standardized: many papers report a rationale without testing faithfulness to the numerical decision.

## 两句话总结
这篇 SLR 把 LLM 在 forecasting/anomaly detection 中的用法拆成直接 prompting、时间序列 foundation model、fine-tuning 和语义辅助等路线，并系统整理数据集、指标与风险。它的核心判断是 LLM 更适合与 news/log/metadata 做混合推理，而纯数值预测和稀有异常检测仍需要专门的时间序列模型、严格的同协议基线和可验证解释。

## Evidence note
This note was written from the full review PDF (arXiv:2402.10350), including the RQs, search/inclusion protocol, Table 3 literature matrix, forecasting/anomaly taxonomy, challenges, risks, and future-directions sections.
