# 388. EEGFormer: Towards Transferable and Interpretable Large-Scale EEG Foundation Model

- **Authors:** Yuqi Chen, Kan Ren, Kaitao Song, Yansen Wang, Yifan Wang, Dongsheng Li, Lili Qiu
- **Venue / year:** AAAI, 2024
- **Paper:** https://arxiv.org/abs/2401.10278
- **Tags:** `foundation-model` `EEG` `self-supervised` `representation` `interpretability`

## Introduction
EEG 信号跨受试者、设备和任务差异很大，传统方法常针对单个数据集和下游任务预训练，难以迁移。医疗应用又要求模型能指出有用脑电 pattern，而不是只给一个 seizure/abnormality label。EEGFormer 研究如何用大规模 compound EEG data 预训练一个 transferable foundation model，并让离散表示支持解释。

## Method / Framework
EEGFormer 的主要结构是：

1. 将固定长度 12 秒、多通道 EEG 经过 channel-independent Transformer encoder；
2. 使用 vector quantizer 将连续 hidden features 映射到 codebook 中的离散 indices；
3. 以 masked/reconstruction 风格的 self-supervised pretraining 学习 EEG 的局部 token、时间模式和跨数据集表示；
4. 下游可 end-to-end fine-tune 或只训练 linear probe；也可以把离散 n-gram indices 接入 naive Bayes 做更直观的 pattern analysis。

离散 codebook 是解释接口：研究者可以统计哪些 EEG token/ngram 与 seizure、artifact、slowing 或 neonatal abnormality 相关，并可视化它们在时间轴上的出现。

## Baselines / Comparisons
下游基线包括 EEGNet、TCN、EEG-GNN、GraphS4mer、BrainBERT，以及从 scratch supervised、linear probing 和 end-to-end fine-tuning 的不同训练设置。数据集包括 TUAB 二分类、TUAR 多类 artifact、TUSL slowing、TUSZ seizure 和 Neonate；指标为 AUROC/AUPRC，multi-class 用 macro AUROC/AUPRC。

## Experiments / Findings
表 1 显示 EEGFormer 在多个 corpus 上表现强且可迁移。以 AUROC 为例，EEGFormer-large 在 TUAB/TUAR/TUSL/TUSZ/Neonate 为 0.872/0.847/0.713/0.878/0.842，EEGFormer-base 为 0.876/0.852/0.679/0.883/0.833；AUPRC 也整体高于 EEGNet、TCN 和 EEG-GNN。摘要强调在 AUPRC 上相对已有方法，Neonate 提升约 15.8%，TUSZ 提升约 14.1%。

只做 linear probe 时已经接近 GraphS4mer 等 supervised baseline，end-to-end fine-tuning 最好，说明预训练表示包含可迁移信息，而不是只在一个任务上过拟合。预训练 epoch 越多，下游性能总体提升。

## Ablation / Error Analysis
论文比较预训练、linear probe、supervised from scratch 和不同模型大小；结果支持“预训练先学到通用离散 EEG pattern，再由任务头读取”的解释。naive Bayes 对 codebook n-gram 的可视化能显示某些 token 在 seizure/normal 时间段的差异，为医疗用户提供比连续 attention 更容易检查的证据。

但离散 token 不等于脑电生理概念。codebook 可能把设备 artifact、受试者差异和疾病模式混在一起；随机切分若包含同一受试者会高估迁移。linear probe 与 end-to-end 差距也说明表示虽可用，但下游任务仍需重塑特征。

## Limitations
EEGFormer 的“interpretability”主要是离散 pattern、n-gram 和 naive Bayes 可视化，不是对 Transformer 内部因果电路的机制解释。数据集和受试者分布、通道对齐、临床标签噪声限制了医学泛化；模型并非 LLM，清单中的 foundation-model 标签是跨模态/基础模型邻近收录。需要专家验证 token 是否对应真实生理现象。

## 两句话总结
EEGFormer 用大规模 EEG 自监督预训练和 vector-quantized discrete representations，把跨数据集迁移能力与可统计的脑电 token pattern 结合起来。它的解释接口比纯连续黑箱更便于可视化，但 token/ngram 仍需临床和因果验证，不能直接当作疾病机制。

## Evidence note
已读取 arXiv 2401.10278 本地 PDF 的摘要、模型结构、下游表格、linear-probe/finetune 对比和 naive Bayes 解释章节；数字来自表 1 和摘要中的提升描述。
