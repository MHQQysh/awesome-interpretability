# 017. LLM Internal States Reveal Hallucination Risk Faced With a Query

- **作者 / venue**：Yejin Bang, Ziwei Ji, Alan Schelten, Anthony S. Hartshorn, Tara Fowler, Cheng Zhang, Nicola Cancedda, Pascale Fung；BlackboxNLP 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.blackboxnlp-1.6/)
- **任务**：在生成前用内部状态估计 query 是否会导致 hallucination

## 1. Introduction：从事后检测转向生成前风险预警

多数 hallucination detector 在模型已经生成答案后才判断真假；这对在线系统代价较高，也无法解释模型在生成前是否已经“知道自己不确定”。论文借鉴人的 self-awareness，提出两个问题：内部状态能否判断 query 是否出现在训练数据中？内部状态能否在生成前预测模型对该 query 的 hallucination risk？

这不是在证明 LLM 有人类式自我意识，而是寻找可操作的 latent signal，用于决定是否需要 retrieval augmentation、拒答或额外验证。

## 2. Method：query hidden state + probing estimator

- 使用 15 类 NLG 任务、700 多个数据集，覆盖 QA、summarization、dialogue 等场景。
- 构造两个标签：seen/unseen query，以及回答后的 hallucination level。
- 取 query 最后 token 在指定 layer 的 activation，训练 probing classifier/estimator，而不是修改生成模型本身。
- 分析不同层、不同 internal state、不同 backbone 的可预测性，并把 estimator 与 PPL、zero-shot prompt、ICL prompt 对比。

## 3. Baseline 与对比

- **Perplexity (PPL)**：传统 uncertainty proxy，检测生成分布是否异常。
- **Zero-shot Prompt**：直接询问 LLM 是否有能力准确回答。
- **In-context Learning Prompt**：给示例，让模型自评风险。
- **Internal-state estimator**：论文方法，使用 hidden activation 的 probe。
- **跨任务与跨模型**：不仅在单个 QA benchmark 上比较，而是在 15 类任务、Llama2/Mistral 等模型上观察是否稳健。

这样的 baseline 设计能区分“模型说自己不确定”和“内部状态中是否存在可读信号”；后者更适合作为系统级 gating feature。

## 4. Findings：内部状态确实携带风险信号

- 识别 query 是否在训练数据中出现，内部状态达到约 **80.28% accuracy**（另一组报告约 80.24%），说明 seen/unseen 信息在 query processing 阶段已可读出。
- 在 15 个 NLG 任务上预测 hallucination risk，平均估计准确率达到 **84.32%**。
- 这些信号在生成前就存在，因此可以在生成前触发 RAG、拒答或人工检查，而不必等答案生成后再做分类。
- 神经元可视化显示，部分 neuron 对 uncertainty/hallucination level 敏感；但这更像一个可训练的表征方向，不等于单个神经元就是“幻觉开关”。

## 5. Ablation 与解释

- 比较最后 token、不同层和不同 internal state，说明层位置会显著影响预测能力。
- 在 seen/unseen 与 hallucination 两个任务上分别训练 estimator，避免把“训练数据记忆”误当成“回答风险”。
- 与 PPL、zero-shot、ICL 比较，internal state estimator 能捕捉 prompt 自评没有显式表达出的风险。
- 作者还讨论 RAG 场景：如果 query-only internal state 已显示高风险，系统可以提前检索，而不是盲目相信生成结果。

## 6. 局限

标签依赖自动 hallucination metrics 和数据集构造；“训练数据见过”不是可完全验证的事实；probe 的跨模型迁移、跨语言迁移和对 adversarial prompts 的鲁棒性仍有限。

**一句话评价**：这篇论文把 hallucination detection 从“读答案”提前到“读 query 处理阶段的内部状态”，为生成前风险 gating 提供了实证基础。
