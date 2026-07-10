# 081. Can Prompt Probe Pretrained Language Models? Understanding the Invisible Risks from a Causal View

> 人工精读笔记：ACL 2022。以下结论依据论文正文、表格和因果分析框架整理，不把标题或摘要当作实验结论。

## 0. 论文信息

- **作者**：Boxi Cao, Hongyu Lin, Xianpei Han, Fangchao Liu, Le Sun
- **来源**：[ACL 2022](https://aclanthology.org/2022.acl-long.398)
- **主题**：prompt-based probing、因果评估、PLM 知识探测的偏差
- **研究对象**：BERT、RoBERTa、GPT-2、BART；主要在 LAMA 式事实知识 probing 上分析

## 1. Introduction：要解决什么问题

Prompt-based probing 把“模型填出答案”当成模型拥有相应知识的证据，例如用“Michael Jordan was born in [MASK]”测试出生地知识。论文指出，这种评估不同于普通监督学习：模型、预训练语料、probe 数据、prompt 和自然语言表述之间存在隐蔽的共同生成因素，因此观察到的分数不一定等于模型真正的知识能力。

作者特别追问两个问题：当前 probing 标准中有哪些系统性偏差？偏差来自 prompt 的语言形式、样本的表达方式，还是预训练语料与测试数据的分布重叠？贡献不是再提出一个“万能正确”的 benchmark，而是用结构因果模型把这些偏差显式化，并给出在假设成立时可以使用的 backdoor adjustment。

## 2. Method / Framework：怎么解决

### 2.1 SCM 建模

论文定义了预训练语料分布 Da、语料 C、模型 M、语言分布 L、关系 R、prompt P、模型加 prompt 得到的 predictor I、probe 数据分布 Db、采样样本 T、自然语言实例 X 和最终表现 E。目标本应是估计模型 M 对表现 E 的真实因果效应，但实际观测的 M-E 相关性被三条 backdoor path 污染。

### 2.2 三种偏差

1. **Prompt preference bias**：语义等价的 prompt 对不同 PLM 的语言偏好不同，比较结果混入了“模型更喜欢哪种句式”。
2. **Instance verbalization bias**：同一个实体或关系用不同自然语言表达时，模型的预测会变，评估变成对措辞的敏感性。
3. **Sample disparity bias**：模型的预训练语料与 probing 数据的重合程度不同；高分可能来自语料覆盖、答案泄漏或领域重叠，而不是更强的抽取能力。

对每条路径，作者给出相应的干预方向：跨多个语义等价 prompt 和 verbalization 做平均，或在模型/数据分布之间做 backdoor adjustment。实际计算中还要枚举有效 prompt、实例表达或相关数据分布，因此代价高，并且依赖“混杂变量已经被正确建模”的假设。

## 3. Baseline / 对比基线

- **传统机器学习评估**：作为概念对照，普通评估假定 train/test 生成机制与被比较的模型假设相互独立；prompt probing 不满足这一点。
- **固定单一 prompt 的 LAMA 评估**：这是被质疑的常规 baseline，容易把句式偏好误当作知识。
- **四种 PLM 横向比较**：BERT/RoBERTa（autoencoder）、GPT-2（autoregressive）、BART（denoising autoencoder），用于测试模型排名是否随 prompt、表达和数据分布变化。
- **随机/干预后的比较**：在样本分布实验中比较原始数据、随机重采样或经过 causal intervention 后的结果，用于隔离预训练语料与 probe 分布的影响。

这里没有把一个新神经网络当作 baseline；核心 baseline 是“原始 prompt probing 评估协议”与“因果调整后的协议”之间的对照。

## 4. Experiments / Findings：结果如何读

### 4.1 Prompt preference

在 language、continent、religion、owned-by 等关系上，四个模型使用语义等价 prompt 时的 P@1 方差显著。表 1 的平均结果显示，BERT-large 原始 LAMA P@1 为 39.08，换 prompt 后 worst/best 为 23.45/46.73，标准差 8.75；RoBERTa-large、GPT2-xl、BART-large 也有类似幅度。更关键的是，模型排名并不稳定：论文报告 96.88% 的关系在 prompt 改变时会出现排名变化。

解释上，这不是简单的“prompt 写得不好”，而是 prompt P 与预训练语言分布 L/C 的共同原因导致 predictor 受语言风格偏好支配。一个模型在某句式上高分，不能直接推出它对该关系的知识更强。

### 4.2 Instance verbalization

同一 probing instance 可以用不同名称、缩写或实体表达来 verbalize。论文展示例如 U.S./America 等表达会诱发不同答案，并在多个 relation 上统计 verbalization stability。稳定性下降意味着测试结果对 X 的表面语言形式敏感，因而单一表达得到的 score 不能代表关系知识本身。

### 4.3 Sample disparity

作者让模型在具有不同预训练语料相关度的条件下进行 probing。表 3 的核心现象是：模型在更接近其预训练分布的 probe 上分数更高；因此不同模型的分数差可能由样本与预训练语料的重合程度造成。这个结果直接挑战“同一 LAMA 分数可以公平排序不同预训练模型”的解释。

### 4.4 Causal intervention

对 1000 个 task samples，原始评估的 rank consistency 很低，文中报告对不同 verbalization 采样时整体一致性只有 5.5%。加入 backdoor adjustment 后，一致性显著提升；表 4 中 BERT-base 的原始/随机/干预结果为 56.4/45.4/86.5，GPT2-medium 为 63.5/40.7/98.2，BART-base 为 63.4/61.6/98.2。数值说明干预可以让比较更稳定，但并不证明它已经恢复了模型能力的“真值”。

## 5. Ablation / 机制解释

论文的“消融”主要是逐个移除或控制三类 backdoor 来源，而不是训练组件消融：改变 prompt 集合检验 prompt bias，改变 verbalization 检验实例 bias，改变预训练/测试分布相关度检验 sample disparity，再对比 causal adjustment 前后排名。这样的设计把每一类偏差与 SCM 中的一条路径对应起来，机制解释比单纯报告平均分更重要。

## 6. Limitations / 局限与复现注意

- Backdoor adjustment 需要完整且可观测的混杂变量；真实预训练语料分布通常未知，不能把论文中的调整公式机械套到所有 benchmark。
- 多 prompt、多 verbalization 和多分布采样会增加评估成本，且 prompt 语义等价本身需要人工或规则判断。
- 论文主要以事实知识 probing 为实验载体；生成、对话和 instruction-following 中的偏差路径可能还要重新建模。
- 结论是“原始 probing 不可靠且需要因果审计”，不是“因果调整后的单一分数绝对正确”。

## 7. 两句话总结

论文要解决的是：prompt probing 的模型排名可能被 prompt、实例措辞以及预训练语料和测试数据的分布重合所左右。它用结构因果模型把三条偏差路径显式化，并通过 backdoor adjustment 提升评估稳定性，实验证明原始排名确实会大幅波动，但调整效果依赖可观测混杂变量和较高评估成本。
