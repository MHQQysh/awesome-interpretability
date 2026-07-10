# 118. MMNeuron: Discovering Neuron-Level Domain-Specific Interpretation in Multimodal Large Language Model

> 人工精读笔记：EMNLP 2024。重点是从 multimodal input 中定位 domain-specific neuron，并验证其对视觉/语言输出的功能影响。

## 0. 论文信息

- **作者**：Jiahao Huo 等
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.387)
- **对象**：多模态 LLM 的视觉/语言 neuron，domain-specific interpretation

## 1. Introduction：要解决什么问题

多模态 LLM 将视觉 encoder、projector 和 language decoder 组合起来，单个 neuron 可能响应对象、属性、空间关系或领域概念。已有 neuron interpretability 多针对文本模型，无法说明某个 neuron 是被图像内容、文本提示还是跨模态对齐激活。

MMNeuron 试图发现并解释 domain-specific neurons，并通过干预检验它们是否真的影响对应领域输出，而不只是与输入表面相关。

## 2. Method / Framework：怎么解决

对选定视觉/文本 domain 构造 prompt-image pairs，收集各 layer 的 neuron activation/importance，筛选对 domain label 或生成 token 区分度高的 neurons。随后用激活样例、token/vocabulary projection 和 multimodal contrast 分析 neuron 的语义。

对 top neurons 做 zeroing/amplification 或参数干预，观察 domain answer probability、caption/VQA prediction 和跨域性能变化。

## 3. Baseline / 对比基线

随机 neuron、文本-only neuron attribution、视觉-only activation、不同 layer/模块的 saliency，以及未干预完整模型。还比较 domain-specific neuron 与通用 neuron 的跨域迁移。

## 4. Experiments / Findings：结果如何读

MMNeuron 能在 multimodal decoder 中找到与特定 domain/object/attribute 相关的 neuron clusters，并显示早期跨模态注入、中层语义组合和深层输出 token 之间的分工。干预 top domain neurons 会改变对应答案概率，随机干预影响小，说明定位具有功能性。

同时，domain neuron 并不等于单一概念：一些 neuron 共享多个视觉语义，且干预 domain neuron 可能损伤其他相关任务。因此解释应报告 activation examples、跨层位置与副作用，而不是只给一个 neuron 名称。

## 5. Ablation / 机制解释

模块/层位置、top-k neuron 数量、domain prompt、amplification vs suppression、随机控制构成主要消融；跨域测试检验 neuron 是专门化还是可迁移。

## 6. Limitations / 局限与复现注意

视觉标注和 domain prompt 的质量会影响 neuron 选择；intervention scale 过大可能产生分布外激活；多模态模型架构与视觉 tokenizer 变化会改变 neuron 位置。

## 7. 两句话总结

MMNeuron 将 neuron-level attribution 扩展到多模态 LLM，定位与领域视觉语义和输出 token 相关的 neuron。激活干预支持其功能作用，但 neuron 仍可能 polysemantic，且视觉输入、文本 prompt 和跨模态 projector 的贡献需要共同审计。
