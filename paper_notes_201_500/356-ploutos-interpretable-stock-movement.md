# 356. Ploutos: Towards Interpretable Stock Movement Prediction with Financial Large Language Model

- **Authors:** Hanshuang Tong, Jun Li, Ning Wu, Ming Gong, Dongmei Zhang, Qi Zhang
- **Venue / year:** arXiv preprint, 2024
- **Paper:** https://arxiv.org/abs/2403.00782
- **Tags:** `financial-llm` `stock-prediction` `rationale-faithfulness` `multimodal`

## Introduction
Stock movement prediction combines numerical price history with text such as news and tweets, but conventional models are opaque and LLM baselines do not reliably produce grounded rationales. The paper focuses on binary up/down movement rather than exact price, and asks whether an LLM can combine heterogeneous experts while explaining which evidence supports the decision.

The work separates prediction from explanation. A technical expert handles alpha/time-series features, a sentiment expert handles news, and a human expert can provide domain judgment; a language model then ranks or synthesizes these views under the current market condition.

## Method / Framework
**PloutosGen** produces expert outputs. The technical expert extracts alpha-formula features and feeds them to a forecasting model; the sentiment expert aggregates news with a debias sentiment-news ratio; the human expert represents optional human thoughts. **PloutosGPT** consumes these predictions and generates bullish and bearish rationales plus a final up/down decision.

Training uses two mechanisms. Rearview-mirror prompting asks GPT-4 to inspect historical cases and select the experts/strategies that explain the observed movement, creating supervised instruction data. Dynamic token weighting computes cosine similarity between token hidden states and verbalizer/type embeddings, emphasizing movement and rationale tokens during fine-tuning from Llama-2. The model is evaluated with accuracy, F1, MCC, faithfulness, and informativeness.

## Baselines / Comparisons
On ACL18 and CIKM18, the baselines are ARIMA, Adv-LSTM, StockNet, DTML, zero-shot GPT-4, Llama-2-7B, and FinMA-7B. The comparison covers classical time series, recurrent/graph/deep stock models, and financial/general LLMs.

For interpretability, the paper compares FinMA, FinGPT, GPT-3, and Ploutos. Faithfulness is judged by GPT-4 using HaluEval-style fact inferability, while informativeness follows TruthfulQA-style scoring. This separates a rationale that is supported by the supplied expert evidence from a generic but plausible sentence.

## Experiments / Findings
The ACL18 dataset has 26,614 balanced samples from 2014-2016, with a 7:1:2 train/validation/test split; CIKM18 has 47 stocks and follows the prior split protocol. On ACL18, Ploutos reaches 61.21% accuracy and 0.205 MCC versus StockNet 58.23/0.081, GPT-4 53.08/0.023, and FinMA 56.28/0.104. On CIKM18 it reaches 59.89% and 0.064, ahead of the listed baselines.

For rationale quality on ACL18, Ploutos obtains 81.24 faithfulness and 96.52 informativeness, above GPT-3 at 77.63/91.58, FinGPT at 72.82/84.76, and FinMA at 59.53/61.32. The result supports the paper's claim that a financial LLM can be made both more predictive and more grounded when it is given explicit expert structure.

## Ablation / Error Analysis
The component ablation removes the sentiment expert (`Ploutos-S`), technical expert (`Ploutos-T`), or rearview-mirror data plus dynamic token weighting (`Ploutos-R`). Their ACL18 accuracies are 59.62, 58.11, and 60.41, respectively, below the full 61.21; the full system also has the best MCC/F1. The results show that numeric-text alignment, both modalities, and the training signal each matter.

Temperature analysis finds a trade-off between decision accuracy and rationale faithfulness. At temperature 0.5 both are highest; lower temperatures over-focus on the movement label and hurt rationales, while higher temperatures reduce predictive consistency. The authors also note that FinMA can be trained on stock instructions yet still underperform, so domain data alone is not enough without the expert/rearview design.

## Limitations
The task is directional and short-horizon, so the results do not establish profitable trading or causal market understanding. GPT-4 is used to generate and judge rationales, which creates evaluator dependence. The human expert is represented as an input channel rather than a rigorously controlled user study, and the paper does not eliminate the risk that a rationale is post-hoc even when its facts are inferable.

## 两句话总结
Ploutos 用技术指标、新闻情绪和人类判断三个专家先产生结构化观点，再用带 rearview-mirror 数据和动态 token 加权的 PloutosGPT 同时预测涨跌并生成理由。它在 ACL18/CIKM18 上超过传统模型和通用/金融 LLM，且理由的 faithfulness 与 informativeness 更高，但仍不能把文本理由等同于真实的市场因果解释。

## Evidence note
本笔记依据 arXiv v1 全文逐节核对；该论文在当前清单中与较早的 Ploutos 条目重复，仍保留本编号独立笔记以保证 README 每行都有对应文档。
