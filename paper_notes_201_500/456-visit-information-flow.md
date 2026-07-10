# 456. VISIT: Visualizing and Interpreting the Semantic Information Flow of Transformers

- **Authors:** Shahar Katz, Yonatan Belinkov
- **Venue / year:** arXiv 2023
- **Paper:** [arXiv:2305.13417](https://arxiv.org/abs/2305.13417)
- **Tags:** `logit-lens` `attention` `visualization` `information-flow`

## Introduction
已有 logit lens 能把 hidden state 投影到词表，但通常只看层间状态或静态权重，难以呈现 attention 内部的动态 memory value 如何影响最终 token。VISIT 的目标是把一次 GPT forward pass 压缩成带语义标签的 flow graph，让研究者看到 query、key、value、head、MLP neuron 和残差之间的交互。

## Method / Framework
作者用 tied unembedding 做 logit-lens projection，并以 top-k token intersection `I_k` 衡量两个向量的语义对齐。attention memory value 先按 attention score 排序；head 输出要经过对应 `W_O` 的 OV projection 后再投影到词表。图中节点代表 hidden state 或高激活 neuron，边按 attention/activation/乘法贡献剪枝，节点颜色表示其对最终预测的排名。

## Baselines / Comparisons
比较直接投影与经过 OV circuit 的投影，比较所有 heads 与 top-norm heads，並用 GPT-2-medium CounterFact factual prompts 做定量分析。定性上复现 GPT-2-small IOI 电路，与 Wang et al. 的 causal circuit table 对照；相关工作包括 Logit Lens、Tuned Lens、MLP key-value memory、NeuronScope 和 attention visualization。

## Experiments / Findings
直接把 `W_V` 输出投影到词表几乎没有对齐；乘 `W_O` 后，attention head 与 block/final output 的语义对齐随深度增加。只有少数高 norm heads 主导拼接输出，说明 attention block 像选择门；被高 attention 的 memory values 与 head 语义更一致。Japan/city 示例中，来自 `Japan` 的 value 携带 Yamato/Japanese concepts，经 head 与 residual 组合后提升 Tokyo 概率。

在 IOI 案例中，VISIT 的图可显示 Name Mover 和 Negative Name Mover 的 token 方向，并揭示 negative head 从 Mary 的 memory value 获得语义。layer norm 的前后投影显示 function words 概率下降、content words 上升，支持 LN 作为 semantic filter；GPT-2-medium 后层每层至少有一个高频激活、高熵的“regularization neuron”。

## Ablation / Error Analysis
去掉 `W_O` 会破坏 attention 语义对齐；只画最大 norm/activation 节点能得到可读图，但可能隐藏低激活却因果重要的成分。VISIT 的图与 IOI 既有研究方向一致，却主要是相关性/语义投影验证，并非完整 causal intervention。

## Limitations
分析集中在英文 GPT 模型和 CounterFact factual statements。top-k token overlap 不能可靠表达同义词关系，最大激活剪枝可能漏掉关键节点；作者明确指出没有用因果技术证明所有流图结论。

## 两句话总结
VISIT 把 attention 的动态 memory value 和 MLP/残差过程投影到词表，再压缩成可交互 flow graph。它用 IOI、layer norm 和 regularization-neuron 案例展示了工具的解释价值，但语义对齐与高激活剪枝仍需 causal validation。

## Evidence note
已读取 projection 定义、attention/value 分析、flow-graph 构造、IOI/LN/neuron 案例及 limitations。
