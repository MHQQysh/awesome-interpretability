# 201. LLMRG: Improving Recommendations through Large Language Model Reasoning Graphs

- **Authors:** Yan Wang, Zhixuan Chu, Xin Ouyang, Simeng Wang, Hongyan Hao et al.
- **Venue:** AAAI 2024
- **Paper:** https://ojs.aaai.org/index.php/AAAI/article/view/29887
- **Tags:** Recommender Systems, LLM Reasoning Graph, Explainability, Self-Verification

## Introduction

传统推荐模型擅长从行为序列找相关性，却很难表达“用户为什么喜欢这些物品”以及用户行为之间的高层因果/语义关系。普通知识图或 sequence graph 只是叠加边，不能自动推理；LLMRG 的问题是能否把用户 profile 与历史行为转成可解释 reasoning graph，再让推荐器利用它而不增加新的用户/物品数据。

## Method / Framework

LLMRG 让 LLM 对用户资料和行为序列执行 chained graph reasoning，生成连接兴趣概念的因果/逻辑链；再通过 divergent extension 扩展候选兴趣，self-verification and scoring 检查链条并过滤低分推理，最后缓存高质量链并用 GNN 编码成 graph embedding，送入原有推荐模型。缓存既降低重复调用，也让推荐依据可以沿图回溯。

## Baselines / Comparisons

实验在 ML-1M、Amazon Beauty 和第三个 Amazon benchmark 上，与 FDSA、BERT4Rec、CL4SRec、DuoRec 以及 DuoRec+sequence graph 比较，并加上 DuoRec 直接拼接 GPT-3.5/GPT-4 但不构建 reasoning graph 的对照。指标是 leave-one-out 下的 HR@5/10 和 NDCG@5/10，同时分析推理链质量与 LLM 调用次数。

## Experiments / Findings

- 在 ML-1M 上，相对 DuoRec，LLMRG(GPT-3.5) 的 HR@5/HR@10/NDCG@5/NDCG@10 分别提升 12.87%/14.10%/23.55%/12.86%；GPT-4 版本提升 14.76%/15.53%/26.01%/13.77%。
- Amazon Beauty 上收益仍存在但较小，说明产品叙事复杂度和可推理关系会影响 LLM 图的价值；直接把 GPT 输出接到推荐器并没有稳定收益，证明图结构与过滤比“调用 LLM”本身更关键。
- 三个模块的案例分析与消融一致：没有 divergent extension 时只能产生显而易见的选择，没有 self-verification 时推荐更投机；缓存 reasoning chains 在约 3000 次推理后可减少约 30% 的 LLM 使用。

## Ablation / Error Analysis

消融比较原始 DuoRec、sequence graph、DuoRec+直接 GPT 和完整 LLMRG，并分别去掉 chained reasoning、divergent extension、self-verification/scoring。错误主要来自 LLM 推理链过度联想、商品关系稀疏、低分链被保留或用户序列太短；Amazon 商品文本较短时，可供图推理的语义空间也更小。

## Limitations

图中“因果”关系仍是 LLM 生成的解释性假设，不是用户真实因果偏好；self-verification 也可能把语言流畅误当作事实有效。LLM 调用成本、缓存陈旧、偏见和隐私问题限制部署，离线 HR/NDCG 不能证明用户能理解或信任图上的解释。

## 两句话总结

LLMRG 用 LLM 把用户行为扩展为经过自验证的个性化 reasoning graph，再用 GNN embedding 增强原推荐器，从相关性推荐走向可追踪的逻辑依据。它在 ML-1M 等数据集提升了排序指标并降低重复调用，但图中的因果语义仍需用户研究和真实干预验证。
