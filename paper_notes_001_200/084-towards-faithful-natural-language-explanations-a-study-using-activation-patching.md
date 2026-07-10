# 084. Towards Faithful Natural Language Explanations: A Study Using Activation Patching in Large Language Models

> 人工精读笔记：EMNLP 2025。本文的核心不是让解释更像人，而是用模型内部的 causal effect 检查解释是否对应真实计算。

## 0. 论文信息

- **作者**：Wei Jie Yeo, Ranjan Satapathy, Erik Cambria
- **来源**：[EMNLP 2025](https://aclanthology.org/2025.emnlp-main.529)
- **代码**：[causal-faithfulness](https://github.com/SenticNet/causal-faithfulness)
- **对象**：6 个开源 decoder-only LLM，规模约 2B--27B；3 个 benchmark

## 1. Introduction：要解决什么问题

LLM 可以生成流畅、可信的 natural language explanation（NLE/CoT），但“听起来合理”不等于它真实反映了模型产生答案时使用的内部路径。已有 faithfulness 测试通常扰动解释文本、输入特征或做 SHAP attribution；作者认为其中一些测试测到的是输出一致性，或因为构造了 OOD 输入而把分布偏移误判成解释的重要性。

论文因此提出一个更贴近 faithfulness 定义的问题：解释中提到的 token 是否在模型内部、相应 layer 上具有与答案相同的 causal attribution？如果解释和答案的内部因果分布不一致，即使答案和解释都很流畅，也不应称为 faithful。

## 2. Method / Framework：怎么解决

### 2.1 Activation patching

对每个问题运行 clean input 和 counterfactual/corrupted input，再把 clean run 中某个 token、某一层的 hidden state patch 回 corrupted run。输出概率的恢复量就是该 hidden state 对答案或解释的 indirect causal effect。遍历 token x layer 位置得到 causal effect matrix。

作者用 **Symmetric Token Replacement（STR）** 代替简单 Gaussian noise：把输入的一组 token 替换为受控的反事实 token，使 corrupted example 仍在自然数据分布附近。这样减少 noise corruption 导致的 OOD 风险。

### 2.2 Causal Faithfulness（CaF）

分别计算答案 token 序列和 NLE token 序列的 activation-patching causal attribution。CaF 比较两者的归一化 attribution distribution，分三个层次：token-and-layer、token-only、layer-only。若解释忠实地反映答案计算，应在重要 token 与重要 layer 上同时呈现一致的 causal effect。

## 3. Baseline / 对比方法

- **SHAP**：输入特征 attribution 的代表，但需要对 feature permutation 做积分，可能引入模型未见过的 OOD 组合。
- **CC-SHAP**：比较答案和解释的 SHAP attribution convergence，是本文最直接的 faithfulness baseline。
- **Corruption-based CoT tests**：删除、替换或扰动 CoT，观察答案性能下降。
- **Simulatability/一致性类测试**：把解释交给模型或检查解释扰动后的输出，能够测一致性，但不直接访问内部计算。
- **STR vs Gaussian Noise（GN）**：这是方法内部的 corruption baseline，用于验证 OOD 控制是否影响结论。

## 4. Experiments / Findings：结果如何读

### 4.1 Plausibility 与 faithfulness 不等价

作者在多模型、多 benchmark 上比较解释的表面 plausibility 与 CaF。核心结果是：alignment-tuned 模型通常同时生成更 plausible、CaF 更高的解释，但高可读性本身不能保证 faithfulness；存在解释听起来合理、却与答案 causal attribution 不匹配的情况。

这一区分很重要：如果只用人类偏好或语言模型打分，可能把 post-hoc rationalization 当成 reasoning trace。

### 4.2 CaF 与既有测试

CaF 与 CC-SHAP 都比较答案/解释 attribution，但 CaF 进一步保留 layer 维度，并通过内部 activation patching 避免 SHAP 的输入组合 OOD。论文报告在不同 benchmark、模型和解释类型之间，已有指标的排序并不完全一致；CaF 作为内部因果指标更直接地对应“解释是否反映模型内部过程”的定义。

### 4.3 OOD 实验

作者将 Gaussian-noise corruption 与 STR 对比：GN 可以改变输入分布，导致 causal attribution 同时反映“模型遇到陌生输入后的不稳定”；STR 则通过可控 token replacement 形成反事实样本。这个实验不是简单证明 STR 分数更高，而是说明 faithfulness 评估必须把 corruption mechanism 纳入解释。

## 5. Ablation / 机制解释

- **三种 CaF 粒度**：token-only 只看解释是否提到关键 token；layer-only 只看计算深度；联合 token-layer 最严格，能排除“token 对了但 layer 错了”的假一致。
- **STR/GN 对照**：检验结论是否来自 OOD perturbation。
- **CaF/CC-SHAP 对照**：检验加入内部层信息是否改变 faithfulness 判断。
- **模型规模与 alignment**：跨约 2B--27B 检查指标随模型/训练阶段的变化，而不是只在单一模型上报告。

## 6. Limitations / 局限与复现注意

- Activation patching 仍依赖 corruption span、patch position 和归一化方式；不同干预设计可能得到不同 causal attribution。
- CaF 需要完整 hidden states，计算成本明显高于文本级 consistency test。
- “与答案 attribution 一致”是 faithfulness 的操作化定义，不等于完整 reverse engineering；复杂模型可能存在多条等价路径。
- 论文覆盖的任务和开源 decoder-only 模型有限，不能直接把 CaF 排名当作所有 explanation benchmark 的最终标准。

## 7. 两句话总结

论文要解决的是流畅的 NLE/CoT 可能只是事后合理化，而现有扰动指标还会混入 OOD 影响的问题。它用受控的 activation patching 计算答案与解释在 token/layer 上的 causal attribution，并提出 CaF，结果显示 alignment 有助于提高解释忠实度，但 plausibility 仍不能替代内部因果证据。
