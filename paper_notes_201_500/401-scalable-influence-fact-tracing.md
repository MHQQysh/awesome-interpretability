# 401. Scalable Influence and Fact Tracing for Large Language Model Pretraining

- **Authors:** Tyler A. Chang, Dheeraj Rajagopal, Tolga Bolukbasi, Lucas Dixon, Ian Tenney
- **Venue / year:** arXiv, 2024
- **Paper:** https://arxiv.org/abs/2410.17413
- **Tags:** `training-data-attribution` `influence-functions` `fact-tracing` `LLM`

## Introduction
LLM 的知识来自海量预训练数据，但给定一个事实回答，很难知道哪些训练样本真正造成了预测。training-data attribution 既可用于版权/数据审计，也可用于解释事实记忆、诊断错误和改进数据集。直接对 160B+ token 预训练做 influence 计算面临梯度存储、Hessian 和全库检索的规模瓶颈。

## Method / Framework
作者提出 **TrackStar**，用梯度近似训练样本对 query 预测的 influence：将每个预训练样本的梯度投影到低维空间，使用 Hessian correction/relative normalization，再做可复用的向量检索。方法把“样本是否蕴含事实”与“样本是否会改变模型对事实的预测”区分开。

实验以 8B 模型和超过 160B tokens 的 C4 预训练规模为核心，使用 T-REx/KILT fact triples。对每个 query，TrackStar 返回 proponents；再用 MRR/recall 测 attribution，用 tail-patching 的单步训练效果测 influence。

## Baselines / Comparisons
基线包括 BM25/经典 model-agnostic retrieval、gradient cosine、gradient dot product、TRAK 以及 TrackStar 的去掉 Hessian、投影或 unit normalization 的变体。closed-set T-REx 与 full C4 检索分别检验“能找到蕴含句”和“找到会改变模型输出的样本”。

## Experiments / Findings
TrackStar 在 influence/tail-patch 上优于 gradient baselines，且在 gradient methods 中 attribution 的 MRR/recall 也最好；但 classical retrieval 仍可能更擅长找直接蕴含事实的 passage。论文的中心 finding 是 attribution 与 influence 不总对齐：最相关的证据句不一定是最能改变模型预测的训练样本。

随着模型规模和训练 token 增加，influence 与 attribution 更接近，说明更成熟的模型可能更依赖事实相关证据；但仍会出现多跳推理、歧义实体、先验和完全无关 passage 被检索的案例。TrackStar 推进了从小子集到 full pretraining influence tracing 的可行性。

## Ablation / Error Analysis
消融表明 Hessian approximation、gradient projection 和 unit normalization 都贡献性能；去掉 correction 的方法仍可工作，但 tail-patch 明显下降。错误案例包括实体名字表面匹配、事实已被模型早期学会、同一事实有多个答案和 query template 歧义。

## Limitations
影响估计仍是近似，不等于真正 retraining 后的 causal effect；单步 tail-patching 只是 influence proxy。C4 全库梯度索引昂贵，检索质量依赖 query template、事实数据和模型 checkpoint。更大模型、非事实知识、长链推理和数据污染场景仍需验证。

## 两句话总结
TrackStar 将投影梯度、Hessian correction 和向量检索组合起来，在大规模预训练上追踪既相关又能影响事实预测的训练样本。它最重要的解释性结论是“证据 attribution”和“因果 influence”不是同一件事，必须分别评估。

## Evidence note
已读取本地 PDF 的摘要、TrackStar 定义、T-REx/C4 评测、消融和错误案例章节；关键规模为 8B 模型与超过 160B 预训练 token。
