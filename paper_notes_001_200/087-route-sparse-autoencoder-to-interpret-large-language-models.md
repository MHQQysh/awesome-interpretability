# 087. Route Sparse Autoencoder to Interpret Large Language Models

> 人工精读笔记：EMNLP 2025。重点核对 RouteSAE 相对单层 SAE、Crosscoder 和随机 routing 的差异。

## 0. 论文信息

- **作者**：Wei Shi, Sihang Li, Tao Liang, Mingyang Wan, Guojun Ma, Xiang Wang, Xiangnan He
- **来源**：[EMNLP 2025](https://aclanthology.org/2025.emnlp-main.346)
- **代码**：[RouteSAEs](https://github.com/swei2001/RouteSAEs)
- **模型**：Llama-3.2-1B-Instruct

## 1. Introduction：要解决什么问题

常规 SAE 一次只在一个 layer 的 residual stream 上训练，因此容易漏掉跨层持续变化的 feature：低层的词级特征可能在浅层最强，高层的句法/长程特征可能在深层才出现。Sparse Crosscoder 可以联合多个层，但为每层设置 encoder/decoder，参数和计算成本随层数增长，而且把多层信息聚合后不容易对某个层做精确干预。

RouteSAE 的问题定义是：能否用一个共享 SAE 加一个很轻量的 router，动态决定当前输入应从哪些 layer 取 activation，同时保持 feature 具有可解释性和可干预性？

## 2. Method / Framework：怎么解决

### 2.1 Shared TopK SAE

基础模块采用 TopK SAE。输入是多个层的 residual stream，router 先对各层 activation 做 sum pooling，再用线性映射得到 layer logits，经 softmax 得到 routing probabilities。对当前样本选择概率最高的层 i*，把该层 activation 送入共享 encoder/decoder。

### 2.2 共享字典与动态 routing

所有层共享同一个 SAE 字典，因此 latent feature 在统一空间中解释；router 只增加少量参数，避免 Crosscoder 为每层复制完整字典。训练目标同时约束 reconstruction/下游 KL divergence 与 sparse TopK representation，使路由选择和 feature 解耦共同优化。

### 2.3 Feature manipulation

因为每次路由保留了最重要层索引，作者可以在选定 feature 上做 activation steering，再将修改后的 activation 注回模型。相比固定某一层的 SAE，RouteSAE 可以根据输入动态定位 feature 更强的 layer。

## 3. Baseline / 对比基线

- **ReLU SAE**：早期稀疏自动编码器，作为基础稀疏表示 baseline。
- **Gated SAE**：用 gate 改善激活控制和 feature shrinkage。
- **TopK SAE**：固定层、直接控制活跃 feature 数，是最重要的同机制 baseline。
- **Crosscoder**：每层独立 encoder/decoder 的多层联合方案，比较跨层覆盖但参数昂贵。
- **Random routing**：给每层相同或随机 routing 权重，检验性能是否来自学习到的动态选择。

## 4. Experiments / Findings：结果如何读

### 4.1 Reconstruction / downstream KL

作者在相同或相近 sparsity 下比较 KL divergence。ReLU 和 Gated SAE 在高稀疏度时重建能力下降更明显；TopK 和 RouteSAE 的 KL 更稳定。RouteSAE 的 learned routing 优于 random routing，说明提升不是仅由多层输入数量带来。

Crosscoder 的多层联合确实能提取跨层 feature，但参数规模约随层数线性增长；RouteSAE 以共享 SAE 保留多层能力，成本更低，并在可比设置下取得更低 KL。

### 4.2 Interpretable feature 数量

作者收集 feature 的最大激活上下文并进行筛选/人工评分。固定 interpretability threshold 时，RouteSAE 提取的 interpretable feature 数量总体高于 ReLU、Gated、TopK 和 Crosscoder；摘要报告在 sparsity=64 时比 baseline SAE 多 22.5%。

Crosscoder 的 feature 数量较低，原因不仅是参数成本，也与多层聚合后 feature 的解释边界更混杂有关。random routing 有时会保留较多 feature，但解释质量不稳定，不能替代 learned router。

### 4.3 Interpretation score

对抽样 feature 的 activation contexts 进行 0--4 分解释评分。摘要报告 sparsity=64 时 RouteSAE 的 interpretability score 比 baseline 高 22.3%；正文还显示在不同 sparsity 下 RouteSAE 基本保持最高或接近最高，说明动态选层帮助 dictionary 对齐 feature 的真实激活层次。

### 4.4 Feature case study 与 steering

RouteSAE 同时发现浅层的词/单位/实体表面形式 feature 和深层的句法/长程模式，例如 “more X than Y” 这类结构。对 feature activation 做干预时，模型输出出现与目标 feature 一致的行为，证明这些 latent 不只是可描述，还能用于控制。

## 5. Ablation / 机制解释

- learned routing vs random routing：验证 router 是否真的学到 layer-feature 对齐。
- sum pooling vs attention aggregation：附录表比较两种聚合；attention 机制并不稳定优于简单 sum pooling，作者因此选择更轻的 sum pooling。
- 不同 sparsity：检验 RouteSAE 的优势是否只出现在单一 K；结果在多种 K 上保持趋势。
- 固定 layer vs dynamic layer：解释为什么高层特征和低层特征不能用同一个固定 layer 最优表示。

## 6. Limitations / 局限与复现注意

- 实验集中在 Llama-3.2-1B-Instruct，不能直接推出大模型或 base model 的同样收益。
- feature interpretability score 依赖人工/语言模型评分协议，主观性与筛选阈值会影响 feature 数量。
- router 选择单一 top layer 可能丢掉需要跨层线性组合的 feature；论文没有证明单层选择在所有机制上足够。
- steering 的行为案例数量有限，尚未系统证明干预不会损害其他任务。

## 7. 两句话总结

RouteSAE 要解决的是单层 SAE 捕捉不到跨层 feature、Crosscoder 又太昂贵且难以干预的问题。它用轻量 router 动态选择 layer，并让所有 layer 共享 TopK SAE 字典，在相同 sparsity=64 下报告 interpretable feature 数量提升 22.5%、解释分数提升 22.3%，但泛化与跨层组合能力仍需进一步验证。
