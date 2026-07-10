# 089. Logit Space Constrained Fine-Tuning for Mitigating Hallucinations in LLM-Based Recommender Systems

> 人工精读笔记：本文把 hallucination 作为推荐系统中的偏好矛盾/事实不一致问题，用 logit-space contrastive constraint 训练 LLM recommender。

## 0. 论文信息

- **作者**：Jianfeng Deng, Qingfeng Chen, Debo Cheng, Jiuyong Li, Lin Liu
- **主题**：LLM 推荐、hallucination、对比式 fine-tuning、logit space
- **来源**：[EMNLP 2025](https://aclanthology.org/2025.emnlp-main.1491)
- **对象**：TALLRec 与 CoLLM 风格系统；LLaMA-7B、Vicuna-7B backbone
- **数据**：movie、book、ML-1M、Amazon-Book；AUC 为主要指标

## 1. Introduction：要解决什么问题

LLM recommender 可以利用自然语言历史记录理解用户偏好，但普通 fine-tuning 容易生成自相矛盾或捏造的推荐。例如模型可能同时判断用户喜欢和不喜欢同一电影，或者把历史中未出现的偏好当成事实。传统推荐模型在 few-shot 条件下又难以获得 LLM 的语义泛化能力。

作者的问题是：能否不重新设计推荐模型，而在 fine-tuning 时直接约束 LLM 的 logit representation，让语义相反的 instruction 得到足够不同、且与用户偏好一致的输出分布？为此提出 LCFT（Logit-space Constrained Fine-Tuning）。

## 2. Method / Framework：怎么解决

### 2.1 正负 instruction pair

对每个用户-物品样本构造语义相反的正/负 instruction，例如“用户喜欢该电影吗？”与“用户不喜欢该电影吗？”。同一历史记录下，模型应对两条 instruction 产生一致而相反的 preference decision。

### 2.2 Logit-space KL constraint

LCFT 保留原始 recommendation/fine-tuning objective，同时加入 KL divergence 项，最大化相反 instruction 的 logit distributions 差异。与 token-level hard label 相比，这个约束直接作用于模型输出分布，减少“两个相反问题给出同一答案”的机会。

LCFT 被集成到 TALLRec 和 CoLLM 两个推荐框架中，分别形成 LCFT-TALLRec 和 LCFT-CoLMF；训练使用 AdamW，实验在 few-shot 和 full-shot 两种数据量下重复多个随机种子。

## 3. Baseline / 对比基线

- **传统推荐模型**：MF、NCF、GRU-BERT 等，检验 LLM 语义能力在少样本下是否有优势。
- **TALLRec/CoLLM 普通 fine-tuning**：不加 LCFT 的原始 LLM recommender，是最关键 baseline。
- **LLM backbone 对照**：LLaMA-7B 与 Vicuna-7B，检查约束是否依赖某个 backbone。
- **Few-shot vs full-shot**：分别检验数据稀缺时的鲁棒性和充足数据下的上限。
- **Ablation w/o LCFT**：在相同架构、数据和训练配置下移除 logit KL 项，隔离真正增益。

## 4. Experiments / Findings：结果如何读

### 4.1 Few-shot

表 1 在 movie/book 数据上比较 16、64、256 shots。LCFT-TALLRec 的 AUC 例如 movie 约为 0.7013、0.7157、0.7436，book 约为 0.6021、0.6195、0.6531；在各 shot 设置下都超过相应普通 fine-tuning 和传统推荐 baseline。关键不是绝对 AUC，而是数据很少时，对比约束仍能提升稳定性。

传统推荐模型在 few-shot 下学习不足，LLM-based recommender 的语义表征更强；但没有 hallucination constraint 的 TALLRec/CoLLM 仍会出现偏好冲突。

### 4.2 Full-shot 与跨系统

表 2 在 ML-1M 和 Amazon-Book full-shot 设置中显示 LCFT-CoLMF 达到约 0.7451/0.7019 和 0.8221/0.6323 的 AUC/NDCG 组合，并在两类数据上优于对照。LCFT 在 LLaMA-7B、Vicuna-7B 及 TALLRec、CoLLM 上都保持收益，说明它更像训练目标层面的约束，而不是某个模型的偶然技巧。

### 4.3 Case study

MovieLens 案例中，普通 fine-tuning 对同一用户既判断喜欢 “The Good, The Bad and The Ugly” 又判断不喜欢；LCFT 输出一致地把该电影判为不喜欢。这个案例直观展示了 KL 对比项如何把正负 instruction 的决策分开，减少逻辑矛盾。

### 4.4 Ablation 与超参数

附录表 4 直接比较 w/o LCFT 与 LCFT-TALLRec：在 movie 16/64/256 shots，AUC 从约 0.6701/0.6713/0.7142 提升到 0.7013/0.7157/0.7436；book 也有一致提升。超参数分析显示 KL 权重过小约束不足，过大则可能损害原始推荐目标，因此需要在偏好区分和任务拟合之间平衡。

## 5. Ablation / 机制解释

论文的主要机制证据是固定 backbone、数据、推荐头和训练流程，只加/去 logit KL 项；再用正负 instruction 的定性案例观察输出是否从矛盾变为一致。它支持的解释是：hallucination 在此处部分表现为 logit space 中相反语义没有被拉开，而不是仅仅是生成文本不流畅。

## 6. Limitations / 局限与复现注意

- 论文把 hallucination 操作化为推荐偏好矛盾，未覆盖事实核验、开放式文本幻觉等更广泛定义。
- 最大化相反 instruction 的 KL 可能导致过度自信或不稳定输出，论文也提醒部署时要谨慎解释 confidence。
- 正负 instruction 的质量直接决定训练信号，模板偏差可能被 LCFT 学到。
- 数据集和 backbone 数量有限，尚未证明在更长历史、冷启动用户或真实在线反馈中同样有效。

## 7. 两句话总结

论文要解决 LLM 推荐系统在历史偏好不足时产生矛盾推荐和幻觉的问题。LCFT 构造语义相反的 instruction pair，并用 KL 项拉开它们的 logit distributions，在 TALLRec/CoLLM、few-shot/full-shot 和多个数据集上都优于普通 fine-tuning，但约束强度和 hallucination 定义仍限制了泛化。
