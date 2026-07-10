# 098. Making Reasoning Matter: Measuring and Improving Faithfulness of Chain-of-Thought Reasoning

> 人工精读笔记：Findings of EMNLP 2024。重点区分 CoT 是否影响最终答案，以及如何训练小模型真正使用中间 reasoning。

## 0. 论文信息

- **作者**：Debjit Paul, Robert West, Antoine Bosselut, Boi Faltings
- **来源**：[Findings of EMNLP 2024](https://aclanthology.org/2024.findings-emnlp.882)
- **方法**：FRODO；因果 mediation analysis、counterfactual preference learning
- **任务**：Quarel、StrategyQA、OpenBookQA、QASC、GSM8K、Causal Understanding

## 1. Introduction：要解决什么问题

Chain-of-thought 能提升性能，但生成的 reasoning trace 可能只是 plausible explanation，最终答案并不真正依赖它。论文把 reasoning faithfulness 定义为：模型是否可靠地使用这些 inference steps 得到最终答案，而不是 CoT 是否看起来连贯。

作者进一步把推理拆成两步：先找规则/事实形成 inference chain，再稳健地用 chain 得到 conclusion。若只训练答案正确，不约束这两步之间的因果关系，模型可能绕开 CoT 或在干预 CoT 后仍给出同一答案。

## 2. Method / Framework：怎么解决

### 2.1 Causal mediation analysis

把输入 X、CoT reasoning R 和最终输出 Y 建成 mediation graph。通过改变输入、替换正确/反事实 reasoning，估计 natural direct effect、natural indirect effect、controlled indirect effect 等，测量 reasoning 对答案的真实影响。

在十二个 LLM 上做 analysis，比较 base、instruction-tuned 和 RLHF 模型。论文发现 CoT 的 causal effect 随任务、模型和 reasoning length 变化很大；模型可能生成正确 reasoning，却没有把它作为答案生成的必要中介。

### 2.2 FRODO

FRODO 包含 inference module 和 reasoning module。inference module 用 implicit causal reward 学习生成正确 reasoning；reasoning module 用 counterfactual and causal preference objective，偏好能导致正确答案的 reasoning，并强化答案对 reasoning 的依赖。

偏好数据不完全依赖人工标注，而是让 LLM 生成 correct 与 counterfactual chains，再用 causal objective 训练小尺寸 LM。

## 3. Baseline / 对比基线

- **SFT**：直接监督最终答案。
- **SFT + CoT**：用 silver rationales 训练标准 CoT。
- **CoT distillation / MARIO**：把大模型 reasoning 蒸馏到小模型。
- **Rainier / preference-based inference**：使用 preference 或 policy optimization 的 reasoning baseline。
- **CL/causal loss variants**：去掉 inference module、reasoner module 或 counterfactual loss，隔离 FRODO 组件。
- **Causal metrics**：NIE/NDE/CIE 不是 baseline，而是评价 CoT 是否真正介导答案的工具。

## 4. Experiments / Findings：结果如何读

### 4.1 CoT 不总是 faithful

因果分析显示不同 LLM 对 reasoning intervention 的敏感性差异很大；有些模型在 reasoning 被替换为 counterfactual 后仍保持原答案，说明 CoT 不是必要路径。RLHF 模型也不一定比 base/instruction 模型更 faithful，alignment 可能提升语言呈现却不保证内部使用。

reasoning length 增长通常使 faithfulness 下降；论文的进一步分析将生成 chains 分为 invalid steps、unnecessary steps 等，说明冗长 CoT 会增加与最终答案无关的文本。

### 4.2 FRODO 性能

在四个主要 reasoning tasks 上，FRODO 相对 standard SFT 或 CoT distillation 取得约 2--3 个百分点的绝对 accuracy 提升。表 4 的小模型结果显示，FRODO 在 StrategyQA、GSM8K、OpenBookQA、QASC 等任务中总体超过四个强 baseline；附加 EntailmentBank 与 Causal Understanding 也有优势。

### 4.3 Robustness/generalization

在 OOD test sets 上，FRODO 比 SFT、SFT+CoT 和较强 reasoning baseline 更稳。其 rationale 的 causal effect 与最终答案更一致，说明提升不只是多生成了文本，而是训练目标确实让 reasoning 成为有用中介。

## 5. Ablation / 机制解释

- 去掉 causal preference loss 会降低 reasoning faithfulness。
- 去掉 inference module 会影响 reasoning 质量，去掉 reasoning module 则影响答案使用 reasoning 的程度。
- 对照 standard SFT 和 SFT+CoT，区分“答案学习”与“中间链被使用”。
- 任务复杂度/链长分析解释为什么某些模型的 CoT causal effect 很低。

## 6. Limitations / 局限与复现注意

- Causal mediation 依赖 intervention 方式、reasoning span 和输出概率定义。
- 反事实 reasoning 由 LLM 生成，可能本身无效，影响 preference 标签。
- FRODO 主要面向小模型和文本 reasoning，是否适合视觉、工具调用或长程 agent chain 尚未验证。
- faithfulness 提升并不等于 reasoning 内容全部正确，仍需外部事实和人工审计。

## 7. 两句话总结

论文发现标准 CoT 常常只是与答案相关而不是答案的真实因果中介，模型在替换 reasoning 后仍能保持答案。FRODO 用隐式 causal reward 生成正确链、再用 counterfactual preference 训练 reasoner，使小模型更依赖真实 reasoning，在多个任务上提升 2--3 个百分点并改善 OOD 泛化。
