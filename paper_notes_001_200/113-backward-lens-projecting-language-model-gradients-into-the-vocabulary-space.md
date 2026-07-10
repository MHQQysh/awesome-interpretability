# 113. Backward Lens: Projecting Language Model Gradients into the Vocabulary Space

> 人工精读笔记：EMNLP 2024。重点是把反向梯度投影到 vocabulary space，回答一个 hidden state/权重对哪些 token 产生支持或抑制。

## 0. 论文信息

- **作者**：Shahar Katz, Omri Avraham 等
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.142)
- **主题**：gradient interpretability、vocabulary projection、token-level causal hypothesis

## 1. Introduction：要解决什么问题

Logit lens 从 hidden activation 向前投影到 vocabulary，可以看到模型在中间层“想输出什么”；但它忽略了反向信号：一个输出目标的 loss gradient 通过哪些 activation/weight 传回来。Backward Lens 旨在把 gradient 解释成词汇方向，帮助定位模型为某个 token 调整了哪些内部计算。

## 2. Method / Framework：怎么解决

作者计算目标 token 对中间表示、MLP neuron 和 attention components 的梯度，再用 unembedding/词汇基把梯度映射到 vocabulary logits。正向 lens 表示“当前 activation 会支持什么”，backward lens 表示“为了该 token，模型内部梯度推动什么方向”。

论文还使用 gradient matrix 的 spanning set/decomposition，避免直接可视化所有维度造成难以阅读的图。通过层、头、neuron 和 token 级对照，分析 backward signal 是否与已知知识/语法现象对齐。

## 3. Baseline / 对比基线

- Logit lens/forward projection。
- Activation magnitude、gradient norm 和 saliency。
- 单 neuron vs spanning-set decomposition。
- GPT-2-medium/GPT-2-large 与不同 target token。
- 直接梯度与 vocabulary-projected gradient。

## 4. Experiments / Findings：结果如何读

Backward Lens 能把复杂梯度转换成可读 token 候选，显示哪些词被支持、哪些词被抑制；在事实和序列预测案例中，它能补充 forward logit lens，解释为什么一个中间状态最终转向某个答案。

梯度矩阵的 spanning-set 分解与逐 neuron 分析得到相近重要方向，说明压缩可视化没有完全丢掉主要信号。该方法也可观察不同 layer/head 对 target token 的分工，但梯度大小本身仍可能被 layer norm、参数尺度和目标选择影响。

## 5. Ablation / 机制解释

比较 forward vs backward、完整矩阵 vs spanning set、gradient norm vs vocabulary increase，并改变目标 token/层位置；这些对照说明 vocabulary projection 提高了可读性，但不自动提供完整 circuit。

## 6. Limitations / 局限与复现注意

梯度是局部线性近似，可能不代表大幅干预后的真实行为；tokenization 和 target choice 会改变词汇方向；projection 依赖 unembedding geometry，不能直接用于没有可访问梯度的闭源模型。

## 7. 两句话总结

Backward Lens 将语言模型的反向梯度投影到 vocabulary space，让研究者看到内部计算如何支持或抑制目标 token。它补充了只看 forward logits 的 logit lens，并通过 spanning-set/逐 neuron 对照提高可读性，但仍需 activation intervention 才能把梯度解释提升为因果结论。
