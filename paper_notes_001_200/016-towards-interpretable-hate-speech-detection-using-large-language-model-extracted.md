# 016. Towards Interpretable Hate Speech Detection using Large Language Model-extracted Rationales

- **作者 / venue**：Ayushi Nirmal, Amrita Bhattacharjee, Paras Sheth, Huan Liu；WOAH/ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.woah-1.17/)
- **任务**：hate speech detection；用 LLM 提取的 features/rationales 训练可解释分类器

## 1. Introduction：检测准确率之外还需要什么

仇恨言论检测是高风险分类任务。仅给出 hate/non-hate 标签很难让审核员知道模型依据了哪些词，也难以判断模型是否把 target identity、方言或平台特征误当成仇恨证据。传统深度模型和 off-the-shelf LLM 通常不透明，而人工 rationale 又昂贵。

SHIELD 的问题定义是：能否让 LLM 只承担“从文本抽取可读证据”的角色，再用这些证据训练检测器，从而同时保留性能和解释性？

## 2. Method：SHIELD 的两阶段结构

1. **LLM feature/rationale extraction**：用 ChatGPT 从输入中抽取可能的 derogatory words、cuss words、hostility cues 和自然语言 rationale；输出结构化 JSON。
2. **Feature embedding**：将抽取的文本 features/rationales 编码成向量。
3. **Hate speech detector**：把原文本表征和 LLM 提取证据结合，训练 HateBERT/其他 encoder 进行分类。
4. **解释对齐评估**：比较 LLM rationales 与人工 annotated rationale 的重叠、相关性和是否覆盖真正的 hate cue。

这里的 LLM 不是最终判决者，而是 feature extractor。这个设计减少了“LLM 直接输出一个不可审计标签”的风险。

## 3. Baseline 与对比

- **HateBERT**：用 Reddit 仇恨内容继续预训练的 BERT，是主要检测基线和 SHIELD 的 detector backbone。
- **HateXplain**：同时使用 hate label、target community 和人类 rationale 的模型，代表带人工解释监督的方法。
- **PEACE**：利用 sentiment/aggression cues 的解释型方法。
- **CATCH**：面向 hate speech 的其他分类/特征方法。
- **ChatGPT one-shot**：测试 LLM 直接零样本/少样本检测，而不是抽取 rationale 后训练。
- **数据**：Implicit Hate Speech Corpus 等多个数据集，主要使用 implicit hate 与 non-hate 做二分类；指标以 accuracy 为主。

## 4. Findings：解释是否牺牲准确率

- ChatGPT 直接做 off-the-shelf detection 只能得到约 **58%-65%** 的准确率区间，说明“语言理解能力强”不等于稳定的仇恨分类器。
- SHIELD 在多个数据集上保持接近或超过基线的检测效果；作者报告在部分设置中比简单基线有最高约 **12.5%** 的性能提升。
- rationale 与 human annotation 的对齐结果显示，LLM 能抽到 derogatory words 和关键短语，但不总能识别隐式仇恨的社会语境；因此论文同时报告性能和 explanation alignment，而不是把所有抽取文本都视为可靠理由。
- SHIELD 在不同 feature embedding model 和 hate detector 选择下仍有变化。表中原始 SHIELD、RoBERTa detector、RoBERTa feature embedder 的结果说明，解释质量与下游表示模型共同决定结果。

## 5. Ablation 与误差来源

- 只用原始文本、不加入 LLM features：检查 rationale augmentation 的真实增益。
- 只加入 words/features、不加入完整 rationale：检查短关键词与长解释的差异。
- 更换 feature embedding model 或 HSD detector：验证方法是否依赖 HateBERT。
- 对比 human rationale 与 LLM rationale：LLM 抽取的解释更容易覆盖表面词，但隐式仇恨、讽刺和 target context 仍可能错。

## 6. 局限与伦理

数据包含冒犯性内容；LLM 提取的 rationale 可能复制有害语言，也可能把模型偏见包装成“解释”。人工 rationale 的覆盖和质量不一致，accuracy 不能取代 subgroup fairness、跨平台泛化和误报分析。

**一句话评价**：SHIELD 的核心贡献是把 LLM 从黑盒分类器改成“可审计证据提取器”，但解释是否真实改善了高风险分类，仍需在人类一致性、跨域泛化和偏差指标上继续验证。
