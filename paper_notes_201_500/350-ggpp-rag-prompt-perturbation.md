# 350. Prompt Perturbation in Retrieval-Augmented Generation based Large Language Models

- **Authors:** Zhibo Hu, Chen Wang, Yanfeng Shu, Hye-Young Paik, Liming Zhu
- **Venue / year:** KDD 2024
- **Paper:** https://arxiv.org/abs/2402.07179
- **Tags:** `RAG` `prompt-attack` `mechanistic-detection` `GGPP` `robustness`

## Introduction
RAG is often treated as a trustworthiness improvement because the generator receives external evidence. This paper asks whether a short, semantically unremarkable prefix inserted into the user prompt can manipulate the retriever so that an attacker-chosen passage is returned, causing a factually wrong answer. The work targets the retrieval side of RAG rather than only the generator's token output.

## Method / Framework
The attack is Gradient Guided Prompt Perturbation (GGPP). It first identifies important tokens in a target passage by masking each token and measuring the embedding change. Those tokens initialize a short adversarial prefix. Gradient-guided coordinate search then changes prefix tokens so the query embedding moves toward the target passage embedding while maintaining distance from the original passage. The process stops when the target enters the top-k retrieval results and the original result leaves.

GGPP works without knowing the entire corpus: it needs the target and original passages and access to the embedding model. The authors also test prompts containing instructions such as “ignore irrelevant information.” By including the instruction in optimization, the prefix can continue to manipulate retrieval.

The paper proposes two detectors. SATe adapts a mechanistic probe to attention patterns and constraint tokens, but examines many attention parameters. ACT is a lightweight logistic-regression probe over last-layer hidden activations. Both are trained on prompts with and without GGPP prefixes to detect whether a perturbation is present.

## Baselines / Comparisons
GGPP is compared with GCG (Greedy Coordinate Gradient) and UAT (Universal Adversarial Trigger), two prompt/jailbreak perturbation methods adapted to RAG. The evaluation uses GPT-J-6B, Mistral-7B, Qwen-7B, and SFR-Embedding-Mistral, across IMDB, Basketball Players, Books, and Nobel Winners datasets. Each dataset uses a different factual constraint and up to 1,000 entries.

## Experiments / Findings
GGPP successfully pushes targeted passages into top-1/top-10 retrieval across all embedding models. In the direct Mistral-7B comparison, Table 3 reports GGPP at 30.6%/41.6% for IMDB, 11.3%/29.6% for Basketball Players, 38.1%/58.8% for Books, and 28.8%/50.0% for Nobel Winners. GCG and UAT are usually near zero or single-digit success in the same table, so GGPP is substantially stronger in this retrieval-attack setting.

The causal-trace analysis shows why the attack works. With GGPP, attention/MLP activations associated with the original constraint disappear and hidden states drift toward the target passage representation. Thus the prefix does not merely fool a string-matching retriever; it changes internal states used to compute embeddings and retrieval rankings.

The detector results are strong. SATe reaches AUROC/F1 values as high as 100%/99.7% for SFR-Embedding-Mistral and 99.9%/98.3% for GPT-J on IMDB. ACT uses far fewer parameters and remains effective, for example F1 96.9% on IMDB with Mistral and 99.4% with SFR-Embedding-Mistral. SATe is usually more accurate, while ACT is much cheaper and faster.

## Ablation / Error Analysis
Prefix initialization is important: starting from the most influential target-passage tokens shortens the embedding-search path and reduces optimization time. The loss-balance parameter peaks around lambda 0.5; too small a value underweights the original/target distance constraint, while too large a value creates imbalance.

The encoder-based SFR-Embedding-Mistral model is not immune. It can obtain high top-1 manipulation rates, but its top-10 improvement is sometimes marginal because of context sensitivity and because the target may already be near the top. The instruction ablation shows that a prompt saying “ignore irrelevant information” reduces success initially, but GGPP can be retrained with that instruction and recover manipulation.

The detector ablation is a resource trade-off. SATe probes all attention weights and has millions of parameters; ACT probes only the last-layer hidden state with roughly 430K parameters in the reported settings. ACT loses some AUROC/F1 on GPT-J/Qwen but is preferable for deployment due to lower training and response time.

## Limitations
The attack assumes access to the embedding model and target/original passages, and experiments use curated factual datasets rather than open-world production stores. Detection probes are trained on GGPP examples and may not generalize to other prompt attacks or unseen embedding architectures. The work demonstrates retrieval manipulation, not every downstream generation failure. Defenses based on activation probes also add engineering cost and can themselves be evaded.

## 两句话总结
GGPP 用 token-importance 初始化和梯度坐标搜索学习很短的 adversarial prefix，把 RAG query embedding 推向攻击者指定的 passage，从而让检索阶段返回错误证据。论文进一步用 SATe/ACT 读取内部 activation 检测这种扰动，说明 RAG 的外部证据链也需要 mechanistic robustness monitoring，而不是只检查最终文本。

## Evidence note
This note was written from the KDD 2024 PDF (arXiv:2402.07179), including Algorithms 1-2, causal-trace figures, datasets/models, Tables 2-4, prefix/lambda ablations, and the conclusion.
