# 025. Mitigating Privacy Seesaw in Large Language Models: Augmented Privacy Neuron Editing via Activation Patching

- **作者 / venue**：Xinwei Wu, Weilong Dong, Shaoyang Xu, Deyi Xiong；Findings of ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.findings-acl.315/)
- **任务**：LLM privacy unlearning；解释和修复 neuron editing 的 privacy seesaw

## 1. Introduction：保护一个隐私可能暴露另一个隐私

神经元级 privacy editing 通常针对一批已知私密样本，抑制承载这些样本的 neurons。但论文发现一个反直觉现象：目标私密文本的泄漏风险下降后，某些未被编辑的 private text 风险反而上升。作者称之为 **Privacy Seesaw**。

这暴露了一个关键问题：编辑的目标不是简单地让某些字符串概率下降，而是要在目标隐私、非目标隐私和模型效用之间保持稳定平衡。

## 2. Method：APNEAP

- **触发因素分析**：目标 privacy data 的数量，以及被编辑 privacy neurons 的数量，是 seesaw 的两个主要 trigger。
- **Augmented private data**：对已收集的 private samples 生成补充私密数据，扩大编辑覆盖范围，减少“只记住少量目标样本”的过拟合。
- **Activation patching**：用 activation patching 改进 privacy neuron identification/editing，使编辑更针对承载隐私的内部状态，而不是粗暴屏蔽整个层。
- **目标**：同时降低 target private data 和 non-target private data 的 leakage，并保持正常任务性能。

## 3. Baseline 与对比

- 传统 privacy neuron editing：只用目标隐私样本做 neuron identification。
- 不同目标数据量：测试 privacy data 数量是否触发 seesaw。
- 不同 edited neuron 数量：测试编辑范围扩大后是否造成非目标风险。
- 与 unlearning、data processing/training/post-processing 方法比较 privacy-utility trade-off。
- 评价包括目标/非目标文本的 extraction risk、模型效用和编辑稳定性。

## 4. Findings

- 目标文本 1-5 的泄漏风险可以被降低，但未编辑的 text 7 风险上升，直接展示 privacy seesaw。
- 只增加编辑神经元并不能解决问题，过度编辑可能把隐私信息重新路由到其他表征。
- APNEAP 通过 augmented data 抑制第一个 trigger，并用 activation-patching based editing 减少第二个 trigger 的副作用。
- 论文报告 APNEAP 在保护目标隐私的同时，对 non-target private data 更稳定；它不是追求某一条字符串的完全删除，而是降低隐私风险的整体波动。

## 5. Ablation 与解释

- 不做 data augmentation：seesaw 更明显，说明编辑集合覆盖不足。
- 不使用 activation patching：privacy neuron 定位不够稳定，容易扩大编辑范围。
- 改变目标数据量和 neuron 数量：结果支持两个 trigger 的因果作用，而非随机现象。

## 6. 局限

隐私泄漏评估受攻击 prompt、抽取策略和 private corpus 影响；activation patching 本身也可能造成 utility loss。真实部署需要考虑未见过的隐私类型、不同语言和多轮攻击。

**一句话评价**：这篇论文把 privacy editing 从“删掉目标样本”推进到“防止编辑引起风险转移”，并把 seesaw 作为必须报告的副作用指标。
