# 346. Learning to Generate Explainable Stock Predictions using Self-Reflective Large Language Models

- **Authors:** Kelvin J. L. Koa, Yunshan Ma, Ritchie Ng, Tat-Seng Chua
- **Venue / year:** WWW 2024
- **Paper:** https://arxiv.org/abs/2402.03659
- **Tags:** `finance` `stock-prediction` `self-reflection` `PPO` `explainability`

## Introduction
Traditional stock predictors can estimate direction but their explanations are often limited to attention weights. LLMs can write natural-language rationales, yet next-day stock movement requires weighing many noisy social-media texts rather than classifying each text independently. The problem is worsened by the lack of expert-written explanation labels for every trading day.

The paper proposes SEP, Summarize-Explain-Predict, to let an LLM teach itself from binary correctness feedback and self-reflection. The key claim is that explanations are not only a presentation layer: forcing the model to state which factors matter can help it make more decisive and better-calibrated predictions.

## Method / Framework
SEP has three stages. The Summarize module extracts factual, informative content from a day's noisy social texts. The Explain module generates a stock direction and a rationale; when the prediction is wrong, a reflective LLM writes a lesson and a high-level correction plan. These reflections become short-term trajectory memory and long-term experience memory for later attempts.

The Predict module is trained in three steps. Correct initial responses provide supervised fine-tuning data. Correct/incorrect response pairs generated during self-reflection train a reward model. PPO then optimizes a Vicuna-7B-v1.5 policy against that reward model with a KL term to prevent mode collapse. At inference, the summarized inputs produce multiple responses and a best-of-N sampler chooses the response with the highest reward score. GPT-3.5 is used for the top stock in each industry and Vicuna-13B for other stocks during summarization/explanation generation.

The framework avoids expert rationale annotation: the only external training signal for the predictor is the known stock movement label, while explanations are generated and improved through reflection. The same idea is extended to portfolio construction, where the reward is portfolio profit rather than binary correctness.

## Baselines / Comparisons
Traditional baselines are VAE+Attention, GRU+Attention, and a Transformer text model. LLM baselines are GPT-3.5, Vicuna-7B, and FinGPT-Forecaster. Prediction uses accuracy and Matthews Correlation Coefficient (MCC), with wrong-format, Neutral, and Mixed answers counted as incorrect. Explanation quality is rated by GPT-4 on relevance, financial metrics, global/industry factors, company developments, temporal awareness, balance, context, clarity, consistency, and sensitivity to updates.

For portfolio construction, the baselines are equal-weight 1/N, S&P 500 market index, Positive-Only equal weighting, GPT-3.5, and Vicuna. Metrics are overall gain, cumulative gain, standard deviation, and annualized Sharpe ratio.

## Experiments / Findings
On stock prediction, SEP with GPT-generated explanations reaches 51.38% accuracy and 0.0302 MCC on all texts for the top stock, and 54.35%/0.0993 after filtering to informative texts. For remaining stocks with Vicuna-generated explanations it reaches 47.59%/0.0203 and 50.57%/0.0508. SEP beats all baselines on MCC; compared with the strongest GRU+Attention baseline, the all-text MCC gains are 0.0177 for the GPT setup and 0.0014 for the Vicuna setup. On informative texts it beats FinGPT-Forecaster by 0.0609 and 0.0129 MCC.

Explanation ratings favor SEP over GPT-3.5 and Vicuna. For example, relevance rises to 5.449, financial metrics to 3.334, global/industry factors to 3.700, clarity/coherence to 6.439, and consistency with information to 6.006 on the 1-7 scale. The model is better at balancing positive and negative evidence instead of simply following the majority sentiment.

For portfolios, SEP reaches overall gain 0.1661, cumulative gain 0.1569, standard deviation 1.792e-2, and Sharpe 1.150, exceeding GPT-3.5 (Sharpe 0.980) and Vicuna (1.104) as well as the non-LLM baselines. The authors interpret this as evidence that the learned ability to weigh textual factors transfers from binary prediction to quantitative allocation.

## Ablation / Error Analysis
Summarization is important. With all summarized texts, the top-stock accuracy/MCC is 51.38/0.0302; using only informative summarized texts improves it to 54.35/0.0993. Random or top-30 raw texts perform worse. Vicuna-generated explanations are more hallucination-prone than GPT-3.5-generated ones and can hurt training.

Removing the explanation module and training only a binary predictor gives 40.84% accuracy on all texts versus 51.38% for SEP. Removing PPO gives 44.08%, while a one-shot PPO variant gives 50.08%; the full best-of-N SEP adds another improvement to 51.38%. The authors report an average 6.9% gain from adding explanations and a 14.8% gain from PPO, showing that both verbal reflection and reward optimization matter.

Self-reflection increases decisive samples by 49.8% and correct samples by 43.2% after three iterations. However, the system can accumulate errors: bad summaries or explanations become future memory and can reinforce a wrong trading story.

## Limitations
Stock movement is noisy and can be driven by factors absent from the text, so explanation quality does not prove causal market understanding. The model may overfit to historical social-media language, and self-generated rationales can hallucinate. PPO, repeated reflection, and best-of-N sampling are costly. Portfolio results are backtests rather than investment guarantees, and the explanation metrics rely on GPT-4 rather than expert financial adjudication.

## 两句话总结
SEP 让 LLM 先总结 noisy social texts，再通过自反思生成正确/错误样本、训练 reward model 和 PPO policy，从而在没有专家 rationale 标注的情况下学习可解释股票预测。它在 MCC、解释质量和 portfolio Sharpe 上都超过基线，但市场噪声、LLM 自生成幻觉与回测评估意味着这些解释不能直接当作真实因果依据。

## Evidence note
This note was written from the WWW 2024 paper PDF, including SEP's self-reflection/PPO pipeline, Tables 1-5, component ablations, explanation ratings, portfolio results, and future-work discussion.
