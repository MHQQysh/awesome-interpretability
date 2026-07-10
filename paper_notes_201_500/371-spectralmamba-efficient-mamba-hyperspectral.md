# 371. SpectralMamba: Efficient Mamba for Hyperspectral Image Classification

- **Authors:** Jing Yao, Danfeng Hong, Chenyu Li, Jocelyn Chanussot
- **Venue / year:** arXiv, 2024
- **Paper:** https://arxiv.org/abs/2404.08489
- **Tags:** `state-space` `Mamba` `hyperspectral` `efficiency` `representation` `interpretability-adjacent`

## Introduction
这篇文章严格来说不是大语言模型可解释性论文，而是将 Mamba/状态空间模型用于高光谱图像分类。它被当前清单收录的价值在于：作者把“光谱序列如何被压缩、哪些空间-光谱信号被保留”变成显式的结构设计和效率分析问题，适合作为可解释的序列建模邻近工作。

高光谱图像同时包含空间信息和数百个连续光谱波段。论文指出三个主要困难：波段维度增大后会出现 Hughes curse；同一材料在不同光照/大气条件下会产生 spectral variability；不同材料又可能出现 spectral confusion。此外，标注样本少、传感器噪声强，使单纯依赖光谱反射率的分类不稳定。CNN 的局部感受野难以处理长程光谱相关性，RNN 难并行，Transformer 的成对注意力又昂贵，因此作者寻找一种既能看长程依赖、又能控制 MACs 和参数量的模型。

## Method / Framework
SpectralMamba 的核心不是把完整高光谱立方体直接拉成长序列，而是在两个层次上做简化建模：

1. **GSSM, gated spatial-spectral merging。** 用高效卷积学习动态 mask，将空间规则性和光谱特异性合并。mask 依赖输入，因此不同样本可以保留不同的空间-光谱响应；作者希望借此减弱光谱可变性和混淆。
2. **Mamba hidden-state modeling。** 合并后的光谱特征送入输入依赖的状态空间模块。与显式 self-attention 不同，模型以线性序列更新方式累积上下文，并让参数随输入选择性响应。
3. **PSS, piece-wise sequential scanning。** 把连续的数百个波段分成若干 piece，再扫描这些较短序列。这样既减少序列长度，也尽量保留局部光谱指纹和较长程上下文。
4. **两种输入形态。** 论文同时研究 pixelwise 和 patchwise 输入，因此可以区分只看单像素光谱与同时利用邻域空间上下文的收益。

从可解释性角度，GSSM mask、piece-wise scan 和输入依赖状态是可检查的中间机制，但论文没有把它们当作人类可读 explanation，也没有证明 mask 等同于因果重要性。

## Baselines / Comparisons
论文在 Houston2013、Longkou、Botswana 和 Augsburg 四个数据集上比较 MLP、CNN、CasRNN、SpectralFormer，并同时报告 overall accuracy (OA)、average accuracy (AA)、Kappa、MACs 和参数量。对比逻辑分成两条：CNN/MLP 代表低成本但上下文有限的模型，RNN/Transformer 代表更强序列建模但计算更重的模型。

Botswana 的表格给出了清楚的效率-效果对比：patchwise SpectralMamba 的 OA/AA/Kappa 为 97.66/98.06/0.9747，MACs 约 36.25M、参数约 34.96K；pixelwise 版本为 91.88/92.70/0.9119，MACs 约 23.39M、参数约 12.33K。作为参照，patchwise CasRNN 为 97.19/97.59/0.9695，但需要 572.31M MACs 和 353.25K 参数；patchwise SpectralFormer 为 94.55/95.32/0.9409，需 685.89M MACs 和 74.03K 参数。这里的结论不是“所有数据集都绝对领先”，而是 SpectralMamba 在保持竞争力的同时显著缩小序列模型的成本。

## Experiments / Findings
主要发现有四个：

- patchwise 输入通常优于 pixelwise，说明空间邻域与光谱序列的联合建模确实重要；但 pixelwise 版本仍以很小参数量提供了可用的光谱分类器。
- 论文的四数据集 radar chart 显示，SpectralMamba 多数情况下同时取得更高 OA 和更低参数/MACs，优于 RNN/Transformer 的典型效率瓶颈。
- PSS 不是单纯的加速技巧。作者报告在合理扫描粒度下，模型可以在保持或提升 OA 的同时减少约 46% MACs 和约 79% 参数，相比不做 piece-wise scanning 的版本更划算。
- 在 Houston2013 上，完整 patchwise 配置的 OA 约为 89.52、AA 约 90.50、Kappa 约 0.8864；表中的组件实验显示，GSSM、PSS 和 Mamba 分别对精度和成本有不同贡献，完整系统的收益来自组合而不是某一个模块单独解决全部问题。

## Ablation / Error Analysis
组件消融以 Houston2013 为例：基础 pixelwise 配置 OA 为 84.62；只加入 GSSM 后 OA 为 85.59；只加入 PSS 后为 86.74；只加入 Mamba 后为 85.73。该结果说明空间-光谱门控和序列状态建模分别提供互补收益，而 PSS 既影响序列表示也影响计算预算。表格排版中的部分组合项存在多列对齐问题，因此不能把每个 MAC/参数数字机械解释成独立因果贡献。

扫描段数 R 的实验比较 2、4、6、8，Houston2013 上 R=6 最优。R 太小会把连续光谱压得过度，R 太大又不能充分降低长度；这揭示了效率和局部光谱保真度之间的折中。误差层面，pixelwise 版本相对 patchwise 的下降也表明：在材料光谱相似时，缺少空间邻域是重要失败来源。

## Limitations
论文主要优化分类准确率和效率，未提供对动态 mask 的人类可读可视化、faithfulness 检验或反事实干预，因此不能直接把 mask 当成“模型为什么这样分类”的充分解释。实验集中在四个高光谱数据集，跨传感器、跨区域和极少标注条件下的稳健性仍需进一步验证。另一个限制是 Mamba 的状态更新虽比 attention 更紧凑，但状态本身仍是高维向量，解释其长期记忆内容需要额外 probing。

## 两句话总结
SpectralMamba 用 GSSM、输入依赖的 Mamba 状态和 piece-wise scanning 处理高光谱分类中的长程光谱依赖，同时压低注意力或递归模型的计算成本。它最有价值的可解释性启示是把空间-光谱选择和序列压缩做成可检查的结构，但论文仍停留在机制透明和效率分析，尚未证明这些中间信号是忠实的因果解释。

## Evidence note
已逐段读取本地 PDF 文本；关键数字来自论文中的 Botswana 对比表、Houston2013 消融表和扫描段数实验。论文主题为高光谱视觉分类，不是 LLM 论文，清单中的 LLM/Hidden 标签属于宽泛的邻近收录。
