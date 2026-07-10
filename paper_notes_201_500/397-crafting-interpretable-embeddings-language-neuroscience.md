# 397. Crafting Interpretable Embeddings for Language Neuroscience by Asking LLMs Questions

- **Authors:** Richard Antonello, Vinamra Benara, Jianfeng Gao, Alexander G. Huth, John X. Morris, Chandan Deep Singh, Ion Stoica
- **Venue / year:** NeurIPS, 2024
- **Paper:** https://doi.org/10.52202/079017-3944
- **Tags:** `LLM` `QA-Emb` `language-neuroscience` `fMRI` `interpretability`

## Introduction
这篇论文是 QA-Emb 在语言神经科学场景中的专门版本/扩展。问题是用现代 LLM 取得强语义表示时，fMRI encoding model 的 feature 往往不可读；研究者需要知道一个 voxel prediction 是由“动作、时间、语法、情绪”哪类文本属性驱动。

## Method / Framework
QA-Emb 的每个维度对应一个由 LLM 回答的 yes/no question。问题集合由 GPT-4 根据 neuroscience、psychology 和数据样例生成，模型回答形成 binary embedding，再用 ridge regression 预测听故事时的 fMRI voxel response。Elastic Net 或 LLM 可以对问题集合做 post-hoc pruning，保留少量与任务相关的 feature。

作者还测量 LLM 是否真的能回答这些问题：在 54 个 diverse binary classification datasets 上评估 GPT-4、LLaMA-3、GPT-3.5 等的 yes/no accuracy。这样把“feature 名称可读”与“feature 的值是否可信”分开。

## Baselines / Comparisons
主要对比 Eng1000 这一 neuroscience interpretable embedding、BERT 等黑箱 embedding、LLaMA embedding，以及没有 task-specific question selection 的全部问题集合。指标是跨 subject/voxel 的 fMRI test correlation；解释质量通过问题数量、cortex weight map 与已有脑区知识的一致性检验。

## Experiments / Findings
在三个受试者听 20 多小时 narrative stories 的数据上，QA-Emb 平均 test correlation 约 0.116，比 Eng1000 高 26%，并略高于 BERT；最紧凑的 29-question QA-Emb 达到约 0.122，对比 Eng1000 约 0.118。问题回归权重在 S02/S03 等受试者上呈现相近脑区选择性，示例问题如 grammatical complexity 和 physical action 能映射到有语言学意义的 cortex pattern。

另一个 finding 是 question-answering 的 reliability 不是均匀的：简单的 fMRI 相关问题表现较好，但更广泛的 binary datasets 中部分问题接近 50% chance。蒸馏到带多分类头的 RoBERTa 可降低逐问题调用成本，且多个问题的聚合能减弱单个 answer 错误。

## Ablation / Error Analysis
候选问题数、Elastic Net pruning、binary vs probabilistic distillation 和是否加入数据相关问题是主要消融。问题越多通常先提高预测，之后出现冗余；只保留少量高权重问题可以同时压缩和提升解释性。

错误分析明确指出：如果 LLM 对“是否包含数学研究”“是否描述物理动作”等问题理解不准，feature 的自然语言名称会给人虚假信心。问答错误、时序延迟、脑信号噪声和跨受试者差异都可能被回归权重误读成语义选择性。

## Limitations
QA-Emb 仍是相关预测模型，不能证明某个问题 feature 是大脑真实计算机制；fMRI 的空间/时间分辨率也有限。问题生成和选择依赖强 LLM，逐维推理成本高；单一 narrative listening setting 不代表阅读、对话和其他语言。论文还提醒可解释模型可能被用于分析或操纵敏感数据，需注意隐私。

## 两句话总结
这篇语言神经科学工作用 LLM 生成的 yes/no questions 构建可读 QA-Emb，让每个 fMRI 预测特征都能被写成一个人类可检查的语言属性，并以少量问题超过 Eng1000。它证明了“可读 feature”可以兼顾预测力，但 feature faithfulness 依赖 LLM 回答准确、问题选择合理和神经科学专家验证。

## Evidence note
已读取 NeurIPS 2024 版本本地 PDF；与 396 条目主题相近，但本条按 README 中独立的 language-neuroscience 版本记录其专门实验和风险边界。
