# 380. RecExplainer: Aligning Large Language Models for Explaining Recommendation Models

- **Authors:** Yuxuan Lei, Jianxun Lian, Jing Yao, Xu Huang, Defu Lian, Xing Xie
- **Venue / year:** KDD, 2024
- **Paper:** https://arxiv.org/abs/2311.10947
- **Tags:** `LLM` `recommendation` `surrogate` `alignment` `rationale` `fidelity`

## Introduction
embedding-based recommender 通常把用户和物品编码成高维向量，再由 black-box scorer 给出下一物品或排序。它们表达能力强，却难以向用户说明“为什么推荐这个”，而传统 surrogate 为了可解释通常必须简单，容易失去对复杂 recommender 行为的 fidelity。RecExplainer 的问题是：能否用一个已有世界知识和语言生成能力很强的 LLM 作为高容量 surrogate，同时让它忠实解释目标推荐模型，而不是凭自己的常识编故事？

## Method / Framework
RecExplainer 以 Vicuna-7B 为基础，通过多任务 fine-tuning 对齐一个 target sequential recommender：

1. **Behavior alignment (RecExplainer-B):** 用用户历史、候选物品和目标模型的行为标签/预测，让 LLM 在语言空间模仿 target model 的 next-item、preference 或分类行为。
2. **Intention alignment (RecExplainer-I):** 将 target model 的 user/item hidden embeddings 投影到 LLM 可接收的表示，让 LLM 学习 latent recommender space 中的意图。它能接触压缩后的模型信息，但不一定保留可读 item metadata。
3. **Hybrid alignment (RecExplainer-H):** 同时使用显式标题/历史文本和 latent embedding，结合行为与意图两条信号。

训练任务包括下一物品/偏好预测、用户兴趣分类、item/user 表示相关任务、history reconstruction 等。推理时模型既预测 target recommender 的结果，也按用户指定的角度生成解释，例如 platform 或 genre。

## Baselines / Comparisons
解释基线包括未对齐的 Vicuna-7B 和 ChatGPT；对齐方法内部比较 RecExplainer-B、-I、-H。推荐 alignment effect 还与原始数据标签、目标模型输出以及不适用某些 embedding 任务的模型做对比。实验使用 Amazon Video Games、Movies & TV、Steam 三个公开数据集，target model 是训练好的 sequential recommender。

解释质量由 GPT-4 和人类专家评分，另有 explanation discriminator 判断文本来自哪一个模型，以及 score predictor 用解释预测 target model 分数，后者用 MSE 衡量 fidelity。这样分别测“文本好不好”“是否像 RecExplainer”“解释是否反映目标模型行为”，避免只报告一项主观质量。

## Experiments / Findings
alignment effect 表明 RecExplainer-H 在三数据集的四类 alignment tasks 上总体最好；没有 alignment 的 Vicuna 和 ChatGPT 能凭语言常识给出合理句子，却不理解 target recommender 的执行偏好。RecExplainer-B 通常优于未对齐模型，但只模仿输出行为；RecExplainer-I 在 embedding 信息压缩严重的任务上可能更弱，说明隐藏向量不是完整可读的用户意图。

GPT-4 explanation score 表 2 为：Vicuna-7B 在 Games/Movies/Steam 为 2.0703/2.0261/2.0341，ChatGPT 为 1.9320/1.8360/1.9560，RecExplainer-B 为 2.3240/2.1360/2.4660，RecExplainer-I 为 1.6653/1.4689/1.3394，RecExplainer-H 为 2.5240/2.2204/2.4920。score predictor 的 MSE 表 3 中，RecExplainer-H 为 1.2786/1.9190/0.3248，明显低于 Vicuna 的 3.6970/2.8373/0.9687 和 ChatGPT 的 3.4803/2.8393/1.0288。

人类评测在 Amazon Video Games 上也支持 hybrid 的总体优势；case study 显示 RecExplainer-B/H 能指出用户偏好 gaming accessories/devices 而非 games，并发现 Xbox-360 engagement 不明显，和 target model 一致。模型还能按 platform 或 genre 改变解释角度，同时保持预测一致，说明 instruction following 和可控解释仍然保留。

## Ablation / Error Analysis
解释任务消融逐个移除训练 task。所有 task 都有贡献，其中 task 3 interest classification 和 task 6 history reconstruction 影响最大：前者帮助模型为 target prediction 提供理由，后者补偿 user embedding 比 item embedding 更严重的信息压缩。图 7 的 GPT score 在去掉 task 3 或 task 6 后下降最明显。

失败案例揭示了 alignment 的边界：RecExplainer-I 可能编造不存在的游戏，因为只从 latent neuron signals 恢复领域信息会丢失实体细节；行为对齐也可能只学到相关输出而不是 target model 的内部原因。discriminator confusion matrix 显示 RecExplainer 解释与其他模型可区分，但“可区分”不等于忠实，MSE 和人工/ GPT-4 评分必须一起看。

## Limitations
RecExplainer 解释的是 recommender 的行为，不是证明它恢复了 target model 的真实内部机制；latent embedding 有信息压缩和不可逆性。实验集中在三个 Amazon/Steam 数据集、Vicuna-7B 和一个 target recommender，跨领域推荐、在线分布漂移和更强基础模型尚未验证。GPT-4 评分与三名人类评分存在协议和成本限制，语言流畅度仍可能影响分数。

## 两句话总结
RecExplainer 通过 behavior、intention 和 hybrid alignment 把 LLM 训练成黑箱推荐模型的高容量语言 surrogate，并同时评估预测对齐、解释质量和分数 fidelity。Hybrid 在三数据集总体最好，但 embedding 压缩、实体幻觉和“模仿行为不等于恢复机制”仍说明它是 faithful behavioral explanation，而不是完整的内部因果解释。

## Evidence note
已读取 arXiv 2311.10947 v2/KDD 2024 本地 PDF；数值来自 alignment 表、GPT-4 explanation 表、score predictor MSE 表、human/case study 与 task ablation。
