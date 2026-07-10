# 381. Transformer-based Language Models and Homomorphic Encryption: An Intersection with BERT-tiny

- **Authors:** Lorenzo Rovida, Alberto Leporati
- **Venue / year:** IWSPA, 2024
- **Paper:** https://doi.org/10.1145/3643651.3659893
- **Tags:** `BERT` `privacy` `homomorphic-encryption` `secure-inference` `interpretability-adjacent`

## Introduction
这篇工作不是 LLM 可解释性论文，而是研究如何在不暴露明文文本的情况下运行 Transformer 语言模型。它与可解释性清单的交集在于：隐私保护的推理会改变模型中间表示、分类输出和调试方式，理解“加密计算是否保留原模型行为”需要可比较的评测。

作者针对的现实问题是 BERT 类模型通常要求服务端访问明文，而医疗、金融或企业文本不应直接交给推理服务。Fully Homomorphic Encryption (FHE) 允许在密文上计算，但 Transformer 的 attention、LayerNorm、非线性和深度会造成巨大的算术和噪声成本。

## Method / Framework
作者实现一个基于 CKKS 的 FHE circuit，将最小的 BERT-tiny 转换为可在加密输入上执行的模型。系统支持两类输出：加密句子 representation，以及加密文本分类结果。为控制 circuit 深度，Layer Normalization 使用预计算近似；其他非线性和操作也用适合 CKKS 的多项式/近似形式，并使用 bootstrapping 延长可计算深度。

## Baselines / Comparisons
核心对比不是多个大模型，而是明文 BERT-tiny 推理与加密/近似实现之间的准确率、误差和计算开销差异。实验使用 Stanford Sentiment Treebank (SST-2)，并检查预计算 LayerNorm、FHE 近似运算和 polynomial approximation 是否改变分类结果。相关工作还对比了早期加密线性模型、CNN、SVM、fastText 和 Transformer secure inference，但本文的重点是 BERT-tiny circuit。

## Experiments / Findings
论文报告在 SST-2 上，预计算 LayerNorm、近似 FHE 操作和多项式近似引入的误差没有造成显著性能损失。也就是说，作者证明了一个小型 Transformer 的“加密 representation + 分类”流程在可接受的任务准确率下可运行，而不是证明大规模 GPT/LLM 已经能高效加密部署。

从解释角度，这个结果支持一种可审计接口：客户端可以保留密文输入，服务端只操作密文，之后对加密的中间 representation 或分类结果再由授权方解密。论文并未把 representation 做成可读解释，也没有测量 saliency/faithfulness。

## Ablation / Error Analysis
误差来源主要是 CKKS 的近似数值、LayerNorm 的预计算以及多项式近似，而不是数据集分类错误的机制分析。论文通过替换或加入这些近似组件观察 SST-2 性能是否下降；其结论是任务损失不显著，但 secure circuit 的延迟、内存和噪声预算仍是主要成本。

## Limitations
BERT-tiny 和 SST-2 远小于现代 LLM 和长上下文生成，不能据此推断 GPT 级模型在 FHE 下实用。论文关注 secure inference，不提供 mechanism interpretability、feature attribution 或 human-readable rationale。预计算 LayerNorm 也依赖输入范围和部署设定，可能影响泛化与安全边界。

## 两句话总结
本文用 CKKS 同态加密实现 BERT-tiny 的密文表示提取和 SST-2 分类，并用预计算 LayerNorm 与多项式近似控制 Transformer circuit 的深度。它说明隐私保护推理可以在小模型上保持任务性能，但加密表示不是自然语言解释，规模、延迟和真正的可解释性仍未解决。

## Evidence note
出版商 PDF 和开放仓储下载在当前环境触发 Cloudflare challenge；已核对开放仓储元数据、摘要和索引文本，未把摘要之外的实验数字写成已验证事实。该条目应保留“全文访问受限”的证据边界。
