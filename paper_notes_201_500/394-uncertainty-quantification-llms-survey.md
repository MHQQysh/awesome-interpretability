# 394. A Survey on Uncertainty Quantification of Large Language Models: Taxonomy, Open Research Challenges, and Future Directions

- **Authors:** Ola Shorinwa, Zhiting Mei, Justin Lidard, Allen Z. Ren, Anirudha Majumdar
- **Venue / year:** ACM manuscript, 2025
- **Paper:** https://arxiv.org/abs/2412.05563
- **Tags:** `survey` `uncertainty` `calibration` `hallucination` `mechanistic-interpretability`

## Introduction
LLM 常以很高置信度输出事实错误内容，且直接问“你有多确定”也可能得到过度自信的文字。用户在医疗、法律、机器人等高风险场景需要的不是一个看似流畅的答案，而是可校准的 uncertainty estimate，用来决定是否检索、拒答、请求人类或继续执行。

## Method / Framework
survey 将 UQ 方法统一成四类：

1. **Token-level UQ:** 使用 token probability、entropy、sequence likelihood 或 logit 统计；需要访问模型输出/架构。
2. **Self-verbalized UQ:** 让 LLM 自报 confidence/uncertainty，部署方便但容易受语言习惯和过度自信影响。
3. **Semantic-similarity UQ:** 多次采样，比较答案语义一致性、聚类和 disagreement，适合黑箱模型但依赖语义相似度和采样预算。
4. **Mechanistic-interpretability UQ:** 读取中间表示、probe、belief/activation 信号，试图获得比最终 token 更有依据的置信度。

应用覆盖 factuality/hallucination detection、chatbot/text generation、问答以及机器人计划和导航。

## Baselines / Comparisons
survey 将 self-report 与 token probability、semantic consistency 和 mechanistic probe 对比。token-level 往往便宜且细粒度，但 next-token confidence 不等于整段事实正确；self-verbalized 很方便但通常不校准；semantic methods 不需白箱访问却要多次生成；mechanistic methods 可能更有解释性，但依赖内部访问和任务特定 probe。

评价应同时看 calibration、selective accuracy/coverage、tightness/conservativeness、OOD/跨域泛化和 interpretability，而不是只看 AUROC。机器人应用还要把 uncertainty 映射到 asking-for-help 或 stopping policy。

## Experiments / Findings
这是 survey，没有统一新模型数字。主要发展流程是从 token likelihood 和经典 calibration，发展到 verbalized confidence、semantic disagreement，再到用 mechanistic interpretability 读中间层信号。作者指出 uncertainty estimation 与 hallucination detection 关系紧密，但 confidence 低不一定是事实错，confidence 高也不代表真值。

综述覆盖的应用显示 UQ 可以让模型 abstain、触发检索、把规划交给人类或选择更安全路径；但 RLHF 可能改变/降低 calibration，jailbreak prompt 还会导致 safeguards 的 confidence miscalibration。

## Ablation / Error Analysis
常见消融包括 sampling 数量、temperature、聚类语义模型、token/sequence aggregation、是否校准 temperature/Platt，以及不同层和 probe 的选择。错误分析包括答案多样但语义等价被误判为不确定、错误答案重复生成导致虚假高置信、prompt wording 改变 self-report、以及多 claim response 中一个总分掩盖局部 uncertainty。

## Limitations
不同论文的 uncertainty 定义、ground truth 和 calibration metric 不一致，survey 无法给出 universal best method。mechanistic UQ 需要白箱模型和额外训练，self-report 的解释性尤其不可靠。应用于机器人还要处理时间、风险和执行后果，不能把离线 confidence 直接当作安全概率。

## 两句话总结
这篇 survey 将 LLM 不确定性分为 token-level、自述、语义相似度和机制解释四条路线，连接 hallucination 检测、校准和机器人决策。它的核心结论是 confidence 必须按 coverage、calibration、OOD 和可解释性共同评估，任何单一 token probability 或自报百分比都不是事实正确性的保证。

## Evidence note
已读取 arXiv 2412.05563 v2 本地 PDF；论文版本显示为 2025 manuscript，README 的 2024 日期可能是预印本日期，笔记按文献内容而非表格日期解释。
