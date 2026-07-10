# 016. Towards Interpretable Hate Speech Detection using Large Language Model-extracted Rationales

> 逐篇阅读记录：第 16 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Ayushi Nirmal, Amrita Bhattacharjee, Paras Sheth, Huan Liu
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.woah-1.17)
- **领域标签**：Probing, Rationale, Evaluation, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 7,225 个词

## 1. Abstract 讲解

- **研究问题**：模型生成的理由可能只是事后合理化，研究需要同时考察理由的可读性、忠实性和对决策的实际解释力。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：Although social media platforms are a prominent arena for users to engage in interpersonal discussions and express opinions, the facade and anonymity offered by social media may allow users to spew hate speech and offens
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction pre-trained language models or other deep neural；1. We propose SHIELD, a framework that lever- speech detection, such features could include cate-；2. We evaluate the goodness of LLM-extracted form this feature extraction, for each input text we；3. Through comprehensive experiments on both list of rationales, list of derogatory language, list；3 Methodology and Experimental posed model on the Implicit Hate Speech Corpus；4 Results and Discussion；7 Limitations；8 Ethical Considerations project (HR001120C0123), and the Office of Naval
- **引言关键线索**：network type models (Sheth et al., 2023b) that are not interpretable or explainable. However, the task Content Warning: This document contains of hate speech detection is a very sensitive task, content that some may find disturbing or and explainability of automated detectors is an es- offensive, including content that is discrimi- sential and desirable feature. Model interpretability native, hateful, or violent in nature. is essential not only for end-user understanding but also for understanding biased predictions, domain shifts, other errors in the prediction, etc. Social media has become a platform of content While incorporating qualities of interpretability sharing and discussions for a varied range of in- directly into deep neural network models such as dividuals with differing cultural and continental pre-trained language model based detectors is chal- * These authors contributed 
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：disdain, or contempt based on their social attributes most of these black-box methods are not in- terpretable or explainable by design. To ad- (e.g., gender, race). In extreme cases, hate speech dress the lack of interpretability, in this paper, may often lead to real world harms such as hate we propose to use state-of-the-art Large Lan- crimes, for example the anti-Asian hate crimes dur- guage Models (LLMs) to extract features in the ing the COVID-19 pandemic (Findling et al., 2022; form of rationales from the input text, to train Han et al., 2023). Therefore, it is essential to have a base hate speech classifier, thereby enabling automatic hate speech detection and moderation faithful interpretability by design. Our frame- in place to maintain the integrity of social media work effectively combines the textual under- standing capabilities of LLMs and the discrim- platforms as well as to mitigate negative impacts inative power of state
- **方法与解释性关系**：该论文主要围绕 `Probing, Rationale, Evaluation, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Baselines we use a, HateBERT3 model. We use, We compare our proposed, SHIELD framework to a, AdamW optimizer, Kingma and Ba, Model training was, GP102, TITAN Xp, GPU with 12 GB, VRAM, PEACE, We further extend our, CATCH, Furthermore, For the Feature Embedding, Sec-, Hate Speech Detector stark
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - tail including the datasets we included, the baseline
  - 3.2 Baselines we use a pre-trained HateBERT3 model. We use
  - We compare our proposed SHIELD framework to a AdamW optimizer (Kingma and Ba, 2014) with
  - variety of different baselines in order to understand a learning rate of 2 脳 10鈭5 . Model training was
  - use the following well-known baseline hate speech GP102 [TITAN Xp] GPU with 12 GB VRAM, and
  - PEACE: We further extend our comparison on rationales, and do these rationales align with

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - We show our proposed SHIELD framework in Feature Embedding Model For the textual fea-
  - 2018)) We see significant overlap and a high se-
  - For some additional analysis on the effect of the in training to improve the efficacy of detection.
  - hate (ElSherief et al., 2018). In order to improve a detector model to enable interpretability in an
  - causal cues (Sheth et al., 2023b). Another study 6 Conclusion and Future Work
  - demonstrated improved performance across not LLM-extracted rationales to augment the training
  - pages 149鈥159. to improve hate speech detection. arXiv preprint

## 6. Conclusion、局限与可复现性

- **结论段落线索**：discussions and express opinions, the facade not always peaceful, they can degenerate into un- and anonymity offered by social media may pleasant altercations and bigoted arguments. Thus, allow users to spew hate speech and offensive social media platforms often become a host for hate content. Given the massive scale of such plat- speech. Hate speech is described as any deliberate forms, there arises a need to automatically iden- and purposeful public communication meant to dis- tify and flag instances of hate speech. Although parage a person or a group by expressing hatred, several hate speech detection methods exist, disdain, or contempt based on their social attributes most of these black-box methods are not in- terpretable or explainable by design. To ad- (e.g., gender, race). In extreme cases, hate speech dress the lack of interpretability, in this paper, may often lead to real world harms such as hate we propose to use state-of-the-art Large Lan- crimes, for example the anti-Asia
- **局限/未来工作线索**：causal cues (Sheth et al., 2023b). Another study 6 Conclusion and Future Work；one dataset. Future work can investigate better au- by employing LLMs in an out-of-the-box manner；pretable hate speech detectors, several limitations superiority, incite racial discrimination, or encour-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Towards Interpretable Hate Speech Detection using Large Language Model-extracted Rationales”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
