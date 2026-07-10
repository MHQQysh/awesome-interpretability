# 366. Knowledge Circuits in Pretrained Transformers

- **Authors:** Yunzhi Yao, Ningyu Zhang, Zekun Xi, Mengru Wang, Ziwen Xu, Shumin Deng, Huajun Chen
- **Venue / year:** NeurIPS 2024
- **Paper:** https://arxiv.org/abs/2405.17969
- **Tags:** `knowledge-circuit` `mechanistic-interpretability` `knowledge-editing` `hallucination`

## Introduction
Knowledge neurons and MLP key-value memories localize facts in Transformer parameters, but treating isolated components as the memory can miss how information flows through attention heads, MLPs, embeddings, and residual connections. This paper defines a **knowledge circuit** as a critical subgraph that jointly expresses a factual, linguistic, social-bias, or commonsense behavior.

The goal is not merely to find where a fact is stored. The authors want to explain how the fact is aggregated, moved to the final token, modified by knowledge editing, lost during hallucination, or supplemented by in-context demonstrations.

## Method / Framework
The model is rewritten as a computation graph whose nodes are input embeddings, attention heads, MLPs, and the output, with residual edges. Causal mediation/edge ablation progressively removes non-critical connections; the remaining graph is the knowledge circuit. Experiments use GPT-2 variants and TinyLLaMA across factual, linguistic, social-bias, and commonsense knowledge.

The analysis tracks information accumulation from early to middle layers and special components such as mover heads and relation heads that transport information to the final token. Knowledge editing is tested with ROME and fine-tuning, while hallucination and in-context learning are analyzed by comparing circuits before and after a wrong answer or a demonstration.

## Baselines / Comparisons
The main conceptual baseline is isolated knowledge-neuron/MLP localization. The circuit comparison is between original and edited models, zero-shot and in-context prompts, and identified heads versus randomly selected heads. ROME is compared with ordinary fine-tuning in terms of where the edited information enters and how it propagates.

For in-context learning, the original zero-shot circuit is compared with the circuit after adding a demonstration. Ablating newly appeared heads is compared with ablating random attention heads to test whether the apparent new path is functionally important.

## Experiments / Findings
Standalone knowledge circuits retain a significant portion of the model's original recall performance, showing that the extracted subgraphs capture useful representations. The information is generally aggregated in early/middle layers and enhanced or transported in later layers; mover heads shift relevant information to the final token, while relation heads capture relational content.

ROME tends to insert edited information primarily at its edited layer, after which mover heads transport it to the residual stream of the last token. Fine-tuning integrates the changed token more directly and exerts a stronger downstream influence. This circuit view helps explain why editing one apparent memory block can have side effects or poor generalization.

For hallucination, the model may accumulate both correct and incorrect candidates but fail to transfer the correct one: in the Malaysia-currency case, an incorrect mover head selects “Malaysian” after both “Ringgit” and the wrong candidate were present. For ICL, new attention heads focus on demonstration context like “comparative form of small is smaller”; ablation reduces the answer probability much more than random-head ablation.

## Ablation / Error Analysis
Edge/node ablation is the discovery and validation ablation. Table 2 shows that removing the identified ICL head drops accuracy more than random-head removal across linguistic, commonsense, bias, and factual cases; for example, the linguistic comparative score is 62.24 with the extra head ablated versus 32.55 for a random head, under the reported setup.

The error analysis emphasizes that a circuit can contain the right knowledge but still produce a wrong answer if the mover selects the wrong candidate. The authors also note that dense graphs, ablation thresholds, and the causal-mediation construction can make the discovered circuit approximate rather than unique.

## Limitations
Circuit discovery is time-intensive and model-specific, and the extracted subgraph depends on the chosen knowledge domain, threshold, and patching scheme. The paper provides a preliminary circuit perspective rather than a complete account of every factual behavior. More efficient mask training, SAE-based discovery, and circuit analysis for larger models remain open.

## 两句话总结
Knowledge Circuits 把事实回忆从“某个 MLP 存了这个知识”扩展为由 embedding、attention、MLP 和 mover/relation heads 协同组成的因果子图，并用它解释编辑、幻觉和 ICL。实验表明错误常发生在信息转移而非知识完全不存在，但电路发现昂贵且依赖模型、阈值和任务，仍不是唯一的真实机制。

## Evidence note
本笔记依据 NeurIPS 2024/arXiv v4 全文逐节核对；示例和 ICL 表 2 数值按原文记录。
