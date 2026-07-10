# 023. The Probabilities Also Matter: A More Faithful Metric for Faithfulness of Free-Text Explanations in Large Language Models

- **作者 / venue**：Noah Siegel, Oana-Maria Camburu, Nicolas Heess, Marisa Pérez-Ortiz；ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.acl-short.49/)
- **任务**：评价 free-text explanation 的 faithfulness，改进只看 top-1 标签变化的指标

## 1. Introduction

如果让 LLM 解释自己的预测，解释可能只是 plausible rationale，并未反映真正的决策过程。常见的 faithfulness 测试会干预解释提到的输入特征，然后看模型 top-1 label 是否改变；作者指出这过于粗糙：概率从 49% 变成 51% 和从 1% 变成 99% 都可能只被记成“label changed”。

论文因此把问题限定为：**解释提到的特征是否比未提到的特征更能改变模型的完整预测分布？**

## 2. Method：概率敏感的 faithfulness metric

- 将 faithfulness 分为 explanatory faithfulness 和 causal faithfulness。
- 对 rationale 提到/未提到的输入特征做 intervention。
- 不仅记录 top-1 label 是否变化，还记录干预前后 class probabilities 的变化幅度。
- 设计对照，防止空解释和“把整段输入原样复制”获得虚假的高分。
- 在 e-SNLI、ECQA、ComVE 等任务和不同规模 LLM 上比较解释忠实度。

## 3. Baseline 与指标问题

- 传统 label-based causal metric：只看 top-1 prediction 是否变化。
- 空解释 baseline：如果指标设计不当，空解释可能被判为极端不忠实，但不能说明非空解释哪些部分真正重要。
- 全输入复制 baseline：重复所有 token 会包含真实重要特征，因此会人为获得高 faithfulness；作者用它说明“覆盖率”和“因果相关性”需要分开。
- 模型规模对比：Llama2 7B/70B 等，用来检验更强模型是否自动生成更忠实 explanation。

## 4. Findings

- 概率级指标能区分仅改变决策边界和真正显著改变模型信念的干预。
- 大模型不一定自动产生忠实解释；在 e-SNLI 和 ComVE 上，Llama2 70B 的 explanation 更忠实，但任务和数据集差异仍然很大。
- ECQA 等复杂任务的 free-text explanation 更容易包含多余文本，导致“解释很长”但与模型重要特征的对应较弱。
- 论文显示 explanatory plausibility 和 causal faithfulness 可以脱钩：人类认为有道理的 explanation 不一定能预测概率变化。

## 5. 重要限制

如果 explanation 是在模型做出最终预测后生成，因果 faithfulness 的解释力本身会受到限制；论文讨论了 chain-of-thought 或 selection inference 等结构约束。概率指标也依赖干预方式、tokenization 和模型输出校准，不能把一个分数当成“真实解释率”。

**一句话评价**：这篇论文把 faithfulness 从二值 label flip 推进到概率分布变化，更接近模型决策强度，但仍需要严格区分解释生成时点和干预协议。
