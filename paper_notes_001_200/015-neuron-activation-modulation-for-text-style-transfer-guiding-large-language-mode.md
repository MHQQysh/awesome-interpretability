# 015. Neuron Activation Modulation for Text Style Transfer: Guiding Large Language Models

- **作者 / venue**：Chaona Kong, Jianyi Liu, Yifan Tang, Ru Zhang；Findings of ACL 2025
- **论文**：[ACL Anthology](https://aclanthology.org/2025.findings-acl.403/)
- **任务**：text style transfer；在保持内容的同时改变 sentiment、formality、authorship、toxicity 等风格

## 1. Introduction：为什么 prompt 不够

LLM 做 style transfer 时经常出现方向不对称：A→B 有效，但 B→A 失败；原因包括训练数据不平衡、模型安全偏好和风格/内容信号纠缠。prompt-based 方法能指导输出，但风格控制粒度有限；fine-tuning 方法需要大量高质量平行或伪平行数据。

论文的核心问题是：**能否在不重新训练 LLM 的情况下，找到与风格相关的神经元，并只调节这些神经元？**

## 2. Method：NAM-TST

### 2.1 Activation Sensitivity Index（ASI）

作者对源风格和目标风格样本做 gradient-based activation difference analysis：观察每个 neuron 的激活、梯度以及两种风格之间的变化。ASI 用来区分三类信号：更偏风格的 neuron、更偏内容的 neuron，以及风格和内容纠缠的 neuron。

### 2.2 生成时调节

在生成过程中，目标风格神经元的 activation 按源/目标风格差异进行调节，形式上类似 `a_k <- a_k + λ Δa_k`；对被识别为 content-sensitive 或 intertwined 的神经元进行保护，避免 style transfer 变成内容重写。

## 3. Baseline 与评价

- **LLaMA-3 zero-shot**：作为未做专门干预的基础模型。
- **Prompt-and-Rerank**：代表强 prompt + 候选重排序的 TST 方法。
- **Zero-shot prompting / prompt-based TST**：检验 NAM-TST 是否优于只改 prompt。
- **四类任务**：sentiment、formality、authorship、toxicity。
- **指标**：style transfer accuracy、BLEU/内容保持、fluency 以及 LEURT 等质量指标。只看风格分类准确率会鼓励模型破坏语义，所以论文同时看内容和流畅度。

## 4. Findings：局部神经元调节的收益

- NAM-TST 在四类风格任务上都取得稳定结果，并且不需要额外 fine-tuning 数据。
- 相比 baseline，作者报告平均提升约 **10%**；在 authorship 任务上平均提升约 **18%**，说明神经元调节对作者风格这种细粒度信号尤其有效。
- 表格中的 NAM-TST 例子显示，它不只是把句子改成目标风格，还能保持较高 content/fluency 指标；这支持“风格与内容在部分神经元上可以被分离控制”的解释。
- 对不同 architecture 的实验表明，找到 style neurons 后进行调节比直接屏蔽整个 layer 更细粒度；但 neuron 选择和 λ 仍然是敏感超参数。

## 5. Ablation 与机制分析

- **不区分 intertwined neurons**：容易出现内容损伤，说明风格/内容不是完全分离的两组神经元。
- **只使用 activation difference、不用 gradient sensitivity**：风格识别会混入内容相关激活，控制更不稳定。
- **改变调节强度 λ**：低强度可能无法完成 style transfer，高强度会牺牲语义和流畅度，存在明显的控制曲线。
- 论文进一步分析 neuron activation frequency 和 style/content sensitivity，试图说明哪些 neuron 是持续编码风格，哪些只是被个别样本激活。

## 6. 局限

方法需要每个 source/target style 的样本来估计激活差异；新风格、跨领域风格和非英语文本是否仍能稳定定位神经元，需要额外验证。style accuracy 也依赖外部分类器，不能完全等同于人类对风格的判断。

**一句话评价**：NAM-TST 把 representation engineering 的思想用于风格迁移，贡献在于证明“找到少量风格相关 neuron 并在推理时调节”可以替代一部分数据昂贵的 fine-tuning。
