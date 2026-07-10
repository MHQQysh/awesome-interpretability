# 010. Anchored Answers: Unravelling Positional Bias in GPT-2’s Multiple-Choice Questions

## 10. Anchored Answers: Unravelling Positional Bias in GPT-2’s Multiple-Choice Questions

- **作者 / 发表**：Ruizhe Li, Yanjun Gao；Findings of ACL 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.findings-acl.124/)
- **方向**：机制可解释性、位置偏置、MLP/attention 干预、logit lens
- **要解决的问题**：GPT-2 在选择题中会过度偏好第一个选项 A，导致预测依赖选项位置而不是内容，损害模型决策的公平性和可靠性。
- **怎么解决**：作者使用 logit lens 追踪导致 A 偏好的中间 logits，分析 MLP 层和值向量以及 attention heads 的贡献。随后更新相关 MLP value vectors，并重校准注意力模式，以最小范围的参数干预削弱 anchored bias。
- **摘要概括**：研究定位了 GPT-2 中与首选 A 偏置相关的内部模块；定向修改这些向量和注意力后，位置偏置得到缓解，并在多个选择题数据集上提升整体准确率。
- **阅读要点**：该工作展示了从失败案例出发的机制分析路径：先构造可重复的偏置，再追踪造成偏置的 logit 来源，最后做局部编辑。它比单纯的 prompt permutation 更接近因果解释，但跨架构泛化仍需要进一步验证。

## 本批小结

- **解释对象的变化**：从输出行为，扩展到层、attention head、MLP value vector、稀疏特征和来源元数据。
- **证据强度的变化**：从相关性 probing，推进到 activation patching、反事实测试、特征激活/抑制和局部参数编辑。
- **应用方向的变化**：解释性已经直接服务于幻觉评测、RAG 可信度、隐私保护、情绪安全和重复生成控制。
- **仍需警惕的限制**：probe accuracy 不等于因果机制；SAE 特征的命名和可分性仍依赖训练与评测设置；单模型、单语言或单数据集上的干预结果不能直接视为普适规律。

## 第 2 批：论文 11-20

**本批领域发展流程**：研究从解释文本和推理链的表面可读性，转向形式化证明、知识图谱和内部神经元的联合验证。

### 11. Faithful and Robust LLM-Driven Theorem Proving for NLI Explanations

- **作者 / 发表**：Xin Quan, Marco Valentino, Louise A. Dennis, Andre Freitas；ACL 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.acl-long.867)
- **领域定位**：Representation, Rationale, Hidden, LLM, Behavior, Explain
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Representation、Rationale、Hidden、LLM、Behavior 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 natural, explanations, play, fundamental, role, inference；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 12. Cross-Lingual Generalization and Compression: From Language-Specific to Shared Neurons

- **作者 / 发表**：Frederick Riemenschneider, Anette Frank；ACL 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.acl-long.661)
- **领域定位**：Probing, Representation, Hidden, LLM, Layer, Analyze
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Probing、Representation、Hidden、LLM、Layer 展开；在不同层或隐状态上训练探针，并比较层间可读性、跨任务稳定性或表征相似性。
- **摘要概括**：摘要所强调的核心线索包括 multilingual, mllms, demonstrated, remarkable, abilities, transfer；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 13. FiDeLiS: Faithful Reasoning in Large Language Models for Knowledge Graph Question Answering

- **作者 / 发表**：Yuan Sui, Yufei He, Nian Liu, X. He, Kun Wang, Bryan Hooi；ACL 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.findings-acl.436)
- **领域定位**：Evaluation, Reasoning, Hallucination, Behavior, Evaluate
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Evaluation、Reasoning、Hallucination、Behavior、Evaluate 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, often, challenged, generating, erroneous, hallucinated；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 14. CoD, Towards an Interpretable Medical Agent using Chain of Diagnosis

- **作者 / 发表**：Junying Chen, Chi Gui, Anningzhe Gao, Ke Ji, Xidong Wang, Xiang Wan, Benyou Wang；ACL 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.findings-acl.740)
- **领域定位**：Evaluation, Reasoning, LLM, Behavior, Control
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Evaluation、Reasoning、LLM、Behavior、Control 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 field, healthcare, undergone, significant, transformation, advent；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 15. Neuron Activation Modulation for Text Style Transfer: Guiding Large Language Models

- **作者 / 发表**：Chaona Kong, Jianyi Liu, Yifan Tang, Ru Zhang；ACL 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.findings-acl.403)
- **领域定位**：Attribution, Evaluation, Hidden, LLM, Activation, Evaluate
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Attribution、Evaluation、Hidden、LLM、Activation 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 text, style, transfer, aims, flexibly, adjust；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 16. Towards Interpretable Hate Speech Detection using Large Language Model-extracted Rationales

- **作者 / 发表**：Ayushi Nirmal, Amrita Bhattacharjee, Paras Sheth, Huan Liu；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.woah-1.17)
- **领域定位**：Probing, Rationale, Evaluation, LLM, Behavior, Detect
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Probing、Rationale、Evaluation、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 although, social, media, platforms, prominent, arena；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 17. LLM Internal States Reveal Hallucination Risk Faced With a Query

- **作者 / 发表**：Ziwei Ji, Delong Chen, Etsuko Ishii, Samuel Cahyawijaya, Yejin Bang, Bryan Wilie, Pascale Fung；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.blackboxnlp-1.6)
- **领域定位**：Probing, Evaluation, Hallucination, Hidden, Layer, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Probing、Evaluation、Hallucination、Hidden、Layer 展开；在不同层或隐状态上训练探针，并比较层间可读性、跨任务稳定性或表征相似性。
- **摘要概括**：摘要所强调的核心线索包括 hallucination, problem, llms, significantly, limits, reliability；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 18. Explainability Meets Text Summarization: A Survey

- **作者 / 发表**：Mahdi Dhaini, Ege Erdoğan, Smarth Bakshi, Gjergji Kasneci；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.inlg-main.49)
- **领域定位**：Rationale, LLM, Behavior, Explain
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Rationale、LLM、Behavior、Explain 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 summarizing, long, pieces, text, principal, task；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 19. Sui Generis: Large Language Models for Authorship Attribution and Verification in Latin

- **作者 / 发表**：Svetlana Gorovaia, Gleb Schmidt, Ivan P. Yamshchikov；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.nlp4dh-1.39)
- **领域定位**：Attribution, Steering, LLM, Behavior, Control
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Attribution、Steering、LLM、Behavior、Control 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 evaluates, performance, llms, authorship, attribution, verification；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 20. Probing the Emergence of Cross-lingual Alignment during LLM Training

- **作者 / 发表**：Hetong Wang, Pasquale Minervini, Edoardo Maria Ponti；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-acl.724)
- **领域定位**：Probing, Representation, Hidden, LLM, Behavior, Detect
- **要解决的问题**：解决模型不同层和隐状态中到底编码了哪些语言、知识或行为信息的问题。
- **怎么解决**：围绕 Probing、Representation、Hidden、LLM、Behavior 展开；在不同层或隐状态上训练探针，并比较层间可读性、跨任务稳定性或表征相似性。
- **摘要概括**：摘要所强调的核心线索包括 multilingual, llms, achieve, remarkable, levels, zero-shot；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 3 批：论文 21-30

**本批领域发展流程**：研究把解释性连接到模型透明度、隐私编辑、幻觉检测与工具化分析，开始形成可复用的诊断流程。

### 21. Mechanistic?

- **作者 / 发表**：Naomi Saphra, Sarah Wiegreffe；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.blackboxnlp-1.30)
- **领域定位**：Mechanistic, Hidden, LLM, Activation, Analyze
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Mechanistic、Hidden、LLM、Activation、Analyze 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 rise, term, mechanistic, interpretability, accompanied, increasing；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 22. LLMCheckup: Conversational Examination of Large Language Models via Interpretability Tools and Self-Explanations

- **作者 / 发表**：Qianli Wang, Tatiana Anikina, Nils Feldhus, Josef van Genabith, Leonhard Hennig, Sebastian Möller；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.hcinlp-1.9)
- **领域定位**：Rationale, LLM, Behavior, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、LLM、Behavior、Explain 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 qianli, wang, tatiana, anikina, nils, feldhus；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 23. The Probabilities Also Matter: A More Faithful Metric for Faithfulness of Free-Text Explanations in Large Language Models

- **作者 / 发表**：Noah Siegel, Oana-Maria Camburu, Nicolas Heess, María Pérez‐Ortiz；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.acl-short.49)
- **领域定位**：Rationale, Reasoning, Hallucination, Behavior, Evaluate
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Rationale、Reasoning、Hallucination、Behavior、Evaluate 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 order, oversee, advanced, systems, important, understand；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 24. Accelerating Sparse Autoencoder Training via Layer-Wise Transfer Learning in Large Language Models

- **作者 / 发表**：Davide Ghilardi, Federico Belotti, Marco Molinari, Jaehyuk Lim；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.blackboxnlp-1.32)
- **领域定位**：Representation, SAE, Hidden, LLM, Layer, Analyze
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Representation、SAE、Hidden、LLM、Layer 展开；结合稀疏自编码器或稀疏特征分解，把混杂激活转成可定位的特征。
- **摘要概括**：摘要所强调的核心线索包括 sparse, autoencoders, saes, gained, popularity, tool；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 25. Mitigating Privacy Seesaw in Large Language Models: Augmented Privacy Neuron Editing via Activation Patching

- **作者 / 发表**：Xinwei Wu, Weilong Dong, Shaoyang Xu, Deyi Xiong；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-acl.315)
- **领域定位**：Mechanistic, Hidden, LLM, Activation, Control
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Mechanistic、Hidden、LLM、Activation、Control 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 protecting, privacy, leakage, remains, paramount, challenge；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 26. IvRA: A Framework to Enhance Attention-Based Explanations for Language Models with Interpretability-Driven Training

- **作者 / 发表**：Sean Xie, Soroush Vosoughi, Saeed Hassanpour；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.blackboxnlp-1.27)
- **领域定位**：Rationale, LLM, Behavior, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、LLM、Behavior、Explain 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 rationale；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 27. Chain-of-Verification Reduces Hallucination in Large Language Models

- **作者 / 发表**：Shehzaad Dhuliawala, Mojtaba Komeili, Jing Xu, Roberta Răileanu, Xian Li, Aslı Çelikyılmaz, Jason Weston；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-acl.212)
- **领域定位**：Analysis, Hallucination, LLM, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Analysis、Hallucination、LLM、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 generation, plausible, incorrect, factual, information, termed；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 28. The Dawn After the Dark: An Empirical Study on Factuality Hallucination in Large Language Models

- **作者 / 发表**：Junyi Li, Jie Chen, Ruiyang Ren, Xiaoxue Cheng, Xin Zhao, Jian‐Yun Nie, Ji-Rong Wen；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.acl-long.586)
- **领域定位**：Analysis, Hallucination, LLM, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Analysis、Hallucination、LLM、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 junyi, chen, ruiyang, xiaoxue, cheng, zhao；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 29. RAGTruth: A Hallucination Corpus for Developing Trustworthy Retrieval-Augmented Language Models

- **作者 / 发表**：Cheng Niu, Yuanhao Wu, Juno Zhu, Siliang Xu, KaShun Shum, Randy Zhong, Juntong Song, Tong Zhang；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.acl-long.585)
- **领域定位**：Analysis, Hallucination, LLM, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Analysis、Hallucination、LLM、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 cheng, yuanhao, juno, siliang, kashun, shum；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 30. Mitigating Hallucinations in Large Vision-Language Models with Instruction Contrastive Decoding

- **作者 / 发表**：Xintong Wang, Jingheng Pan, Ding Liang, Chris Biemann；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-acl.937)
- **领域定位**：Evaluation, MLLM, Hallucination, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Evaluation、MLLM、Hallucination、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 vision-language, lvlms, increasingly, adept, generating, contextually；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 4 批：论文 31-40

**本批领域发展流程**：研究沿着 steering、hallucination、bias 和 causal patching 展开，重点从定位表征推进到修改表征。

### 31. Steering Llama 2 via Contrastive Activation Addition

- **作者 / 发表**：Nina Rimsky, Nick Gabrieli, Julian Schulz, Meg Tong, Evan Hubinger, Alexander Turner；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.acl-long.828)
- **领域定位**：Steering, Hidden, Behavior, Control
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Steering、Hidden、Behavior、Control 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 nina, rimsky, nick, gabrieli, julian, schulz；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 32. Unified Hallucination Detection for Multimodal Large Language Models

- **作者 / 发表**：Xiang Chen, Chenxi Wang, Yida Xue, Ningyu Zhang, Xiaoyan Yang, Qiang Li, Yue Shen, Lei Liang, et al.；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.acl-long.178)
- **领域定位**：Analysis, MLLM, Hallucination, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Analysis、MLLM、Hallucination、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 xiang, chen, chenxi, wang, yida, ningyu；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 33. UHGEval: Benchmarking the Hallucination of Chinese Large Language Models via Unconstrained Generation

- **作者 / 发表**：Xun Liang, Shichao Song, Simin Niu, Zhiyu Li, Feiyu Xiong, Bo Tang, Yezhaohui Wang, Dawei He, et al.；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.acl-long.288)
- **领域定位**：Evaluation, Hallucination, LLM, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Evaluation、Hallucination、LLM、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 liang, shichao, song, simin, zhiyu, feiyu；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 34. Angry Men, Sad Women: Large Language Models Reflect Gendered Stereotypes in Emotion Attribution

- **作者 / 发表**：Flor Miriam Plaza-del-Arco, Amanda Cercas Curry, Alba Curry, Gavin Abercrombie, Dirk Hovy；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.acl-long.415)
- **领域定位**：Attribution, LLM, Behavior, Analyze
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Attribution、LLM、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 flor, miriam, plaza-del-arco, amanda, cercas, curry；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 35. Enhancing Semantic Consistency of Large Language Models through Model Editing: An Interpretability-Oriented Approach

- **作者 / 发表**：Jingyuan Yang, Dapeng Chen, Yajing Sun, Rongjun Li, Zhiyong Feng, Wei Peng；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-acl.199)
- **领域定位**：Evaluation, Hidden, LLM, Activation, Control
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Evaluation、Hidden、LLM、Activation、Control 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 tends, generate, inconsistent, sometimes, contradictory, outputs；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 36. Transformer-specific Interpretability

- **作者 / 发表**：Hosein Mohebbi, Jaap Jumelet, Michael Hanna, Afra Alishahi, Willem Zuidema；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.eacl-tutorials.4)
- **领域定位**：Analysis, Transformer, Behavior, Analyze
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Analysis、Transformer、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 hosein, mohebbi, jaap, jumelet, michael, hanna；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 37. Are self-explanations from Large Language Models faithful?

- **作者 / 发表**：Andreas Nygaard Madsen, Sarath Chandar, Siva Reddy；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-acl.19)
- **领域定位**：Attribution, Rationale, Reasoning, Hallucination, Behavior, Explain
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Attribution、Rationale、Reasoning、Hallucination、Behavior 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 instruction-tuned, llms, excel, many, tasks, will；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 38. Direct Evaluation of Chain-of-Thought in Multi-hop Reasoning with Knowledge Graphs

- **作者 / 发表**：Thi Thanh Huong Nguyen, Linhao Luo, Fatemeh Shiri, Dinh Phung, Yuan-Fang Li, Thuy-Trang Vu, Gholamreza Haffari；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-acl.168)
- **领域定位**：Rationale, Evaluation, Reasoning, LLM, Behavior, Explain
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, demonstrated, strong, reasoning, abilities, when；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 39. Explainable Depression Detection Using Large Language Models on Social Media Data

- **作者 / 发表**：Yuxi Wang, Diana Inkpen, Prasadith Kirinde Gamaarachchige；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.clpsych-1.8)
- **领域定位**：Rationale, LLM, Behavior, Detect
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、LLM、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 rapid, growth, user, interaction, different, social；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 40. Investigating Layer Importance in Large Language Models

- **作者 / 发表**：Yang Zhang, Yanfei Dong, Kenji Kawaguchi；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.blackboxnlp-1.29)
- **领域定位**：Attribution, Rationale, LLM, Layer, Detect
- **要解决的问题**：解决模型不同层和隐状态中到底编码了哪些语言、知识或行为信息的问题。
- **怎么解决**：围绕 Attribution、Rationale、LLM、Layer、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 llms, gained, increasing, attention, prominent, ability；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 5 批：论文 41-50

**本批领域发展流程**：研究进一步细分语言、知识、层重要性与推理结构，开始比较不同模块对行为的具体贡献。

### 41. LM Transparency Tool: Interactive Tool for Analyzing Transformer Language Models

- **作者 / 发表**：Igor Tufanov, Karen Hambardzumyan, Javier Ferrando, Elena Voita；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.acl-demos.6)
- **领域定位**：Representation, Hidden, LLM, Layer, Analyze
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Representation、Hidden、LLM、Layer、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 transparency, tool, lm-tt, open-source, interactive, toolkit；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 42. Spectral Filters, Dark Signals, and Attention Sinks

- **作者 / 发表**：Nicola Cancedda；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.acl-long.263)
- **领域定位**：Representation, Rationale, Lens, Hidden, LLM, Layer, Explain
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Representation、Rationale、Lens、Hidden、LLM 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 projecting, intermediate, representations, onto, vocabulary, increasingly；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 43. Understanding and Patching Compositional Reasoning in LLMs

- **作者 / 发表**：Z. Li, Gangwei Jiang, Hong Xie, Linqi Song, Defu Lian, Ying Wei；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-acl.576)
- **领域定位**：Representation, Lens, Reasoning, Hidden, Layer, Control
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Representation、Lens、Reasoning、Hidden、Layer 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 llms, marked, revolutonary, shift, falter, when；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 44. MindMap: Knowledge Graph Prompting Sparks Graph of Thoughts in Large Language Models

- **作者 / 发表**：Yilin Wen, Zifeng Wang, Jimeng Sun；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.acl-long.558)
- **领域定位**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Evaluation、Reasoning、Hallucination、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, achieved, remarkable, performance, natural, understanding；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 45. Investigating Cultural Alignment of Large Language Models

- **作者 / 发表**：Badr AlKhamissi, Muhammad ElNokrashy, Mai AlKhamissi, Mona Diab；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.acl-long.671)
- **领域定位**：Probing, Evaluation, Reasoning, LLM, Behavior, Analyze
- **要解决的问题**：解决模型记忆、偏见或社会风险难以从内部状态和输出行为中诊断的问题。
- **怎么解决**：围绕 Probing、Evaluation、Reasoning、LLM、Behavior 展开；在不同层或隐状态上训练探针，并比较层间可读性、跨任务稳定性或表征相似性。
- **摘要概括**：摘要所强调的核心线索包括 intricate, relationship, between, culture, long, been；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 46. The Impact of Reasoning Step Length on Large Language Models

- **作者 / 发表**：Mingyu Jin, Qinkai Yu, Dong Shu, Haiyan Zhao, Wenyue Hua, Yanda Meng, Yongfeng Zhang, Mengnan Du；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-acl.108)
- **领域定位**：Rationale, Evaluation, Reasoning, LLM, Behavior, Analyze
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 chain, thought, significant, improving, reasoning, abilities；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 47. Can LLMs Learn from Previous Mistakes? Investigating LLMs’ Errors to Boost for Reasoning

- **作者 / 发表**：Yongqi Tong, Dawei Li, Sizhe Wang, Yujia Wang, Fei Teng, Jingbo Shang；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.acl-long.169)
- **领域定位**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, demonstrated, striking, reasoning, capability, recent；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 48. Properties and Challenges of LLM-Generated Explanations

- **作者 / 发表**：Jenny Kunz, Marco Kuhlmann；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.hcinlp-1.2)
- **领域定位**：Rationale, Reasoning, LLM, Behavior, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、Reasoning、LLM、Behavior、Explain 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 self-rationalising, capabilities, llms, been, explored, restricted；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 49. LANDeRMT: Dectecting and Routing Language-Aware Neurons for Selectively Finetuning LLMs to Machine Translation

- **作者 / 发表**：Shaolin Zhu, Leiyu Pan, Bo Li, Deyi Xiong；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.acl-long.656)
- **领域定位**：Analysis, Hidden, LLM, Behavior, Detect
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Analysis、Hidden、LLM、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 recent, advancements, llms, shown, promising, results；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 50. Leveraging LLM Reasoning Enhances Personalized Recommender Systems

- **作者 / 发表**：Alicia Y. Tsai, Adam Kraft, Long Jin, Chenwei Cai, Anahita Hosseini, Taibai Xu, Zemin Zhang, Lichan Hong, et al.；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-acl.780)
- **领域定位**：Rationale, Evaluation, Reasoning, LLM, Behavior, Analyze
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 recent, advancements, showcased, potential, llms, executing；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 6 批：论文 51-60

**本批领域发展流程**：研究从单一语言模型分析扩展到视觉语言模型、摘要、评测和数据模式解释，解释目标更加多模态。

### 51. Language-Specific Neurons: The Key to Multilingual Capabilities in Large Language Models

- **作者 / 发表**：Tianyi Tang, Wenyang Luo, Haoyang Huang, Dongdong Zhang, Xiaolei Wang, Xin Zhao, Furu Wei, Ji-Rong Wen；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.acl-long.309)
- **领域定位**：Analysis, Hidden, LLM, Behavior, Analyze
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Analysis、Hidden、LLM、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 tianyi, tang, wenyang, haoyang, huang, dongdong；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 52. Characterizing Large Language Models as Rationalizers of Knowledge-intensive Tasks

- **作者 / 发表**：Aditi Mishra, Sajjadur Rahman, Kushan Mitra, Hannah Kim, Estevam Hruschka；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-acl.484)
- **领域定位**：Rationale, Evaluation, Hallucination, LLM, Behavior, Analyze
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Hallucination、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, proficient, generating, fluent, text, minimal；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 53. Neurons in Large Language Models: Dead, N-gram, Positional

- **作者 / 发表**：Elena Voita, Javier Ferrando, Christoforos Nalmpantis；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-acl.75)
- **领域定位**：Analysis, Hidden, LLM, Layer, Detect
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Analysis、Hidden、LLM、Layer、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 analyze, family, lightweight, manner, done, single；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 54. SPIN: Sparsifying and Integrating Internal Neurons in Large Language Models for Text Classification

- **作者 / 发表**：Difan Jiao, Yilun Liu, Zhenwei Tang, Daniel Matter, Jürgen Pfeffer, Ashton Anderson；ACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-acl.277)
- **领域定位**：Analysis, Hidden, LLM, Behavior, Analyze
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Analysis、Hidden、LLM、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 analysis；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 55. Plausible May Not Be Faithful: Probing Object Hallucination in Vision-Language Pre-training

- **作者 / 发表**：Wenliang Dai, Zihan Liu, Ziwei Ji, Dan Su, Pascale Fung；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.eacl-main.156)
- **领域定位**：Probing, Evaluation, MLLM, Hallucination, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Probing、Evaluation、MLLM、Hallucination、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 large-scale, vision-language, pre-trained, prone, hallucinate, non-existent；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 56. Inseq: An Interpretability Toolkit for Sequence Generation Models

- **作者 / 发表**：Gabriele Sarti, Nils Feldhus, Ludwig Sickert, Oskar van der Wal；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.acl-demo.40)
- **领域定位**：Attribution, Evaluation, Hallucination, Behavior, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Attribution、Evaluation、Hallucination、Behavior、Explain 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 past, natural, processing, interpretability, focused, mainly；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 57. A Multitask, Multilingual, Multimodal Evaluation of ChatGPT on Reasoning, Hallucination, and Interactivity

- **作者 / 发表**：Yejin Bang, Samuel Cahyawijaya, Nayeon Lee, Wenliang Dai, Dan Su, Bryan Wilie, Holy Lovenia, Ziwei Ji, et al.；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.ijcnlp-main.45)
- **领域定位**：Evaluation, MLLM, Reasoning, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Evaluation、MLLM、Reasoning、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 yejin, bang, samuel, cahyawijaya, nayeon, wenliang；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 58. Med-HALT: Medical Domain Hallucination Test for Large Language Models

- **作者 / 发表**：Ankit Pal, Logesh Kumar Umapathi, Malaikannan Sankarasubbu；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.conll-1.21)
- **领域定位**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Evaluation、Reasoning、Hallucination、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 research, focuses, challenges, posed, hallucinations, llms；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 59. Explaining How Transformers Use Context to Build Predictions

- **作者 / 发表**：Javier Ferrando, Gerard I. Gállego, Ioannis Tsiamas, Marta R. Costa‐jussà；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.acl-long.301)
- **领域定位**：Attribution, Rationale, Transformer, Layer, Explain
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Attribution、Rationale、Transformer、Layer、Explain 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 generation, produce, words, based, previous, context；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 60. The Eval4NLP 2023 Shared Task on Prompting Large Language Models as Explainable Metrics

- **作者 / 发表**：Christoph Leiter, Juri Opitz, Daniel Deutsch, Yang Gao, Rotem Dror, Steffen Eger；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.eval4nlp-1.10)
- **领域定位**：Rationale, Evaluation, LLM, Behavior, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、Evaluation、LLM、Behavior、Explain 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 generative, llms, seen, many, breakthroughs, over；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 7 批：论文 61-70

**本批领域发展流程**：研究以知识神经元、潜变量和因果干预为主线，尝试把分布式表征拆成可编辑的局部机制。

### 61. Transformer-based Prediction of Emotional Reactions to Online Social Network Posts

- **作者 / 发表**：Irene Benedetto, Moreno La Quatra, Luca Cagliero, Luca Vassio, Martino Trevisan；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.wassa-1.31)
- **领域定位**：Rationale, Transformer, Behavior, Explain
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Rationale、Transformer、Behavior、Explain 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 emotional, reactions, online, social, network, posts；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 62. Revisiting Relation Extraction in the era of Large Language Models

- **作者 / 发表**：Somin Wadhwa, Silvio Amir, Byron Wallace；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.acl-long.868)
- **领域定位**：Rationale, Reasoning, LLM, Behavior, Explain
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Rationale、Reasoning、LLM、Behavior、Explain 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 flan-t, capable, few-shot, setting, supervising, fine-tuning；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 63. Should You Mask 15% in Masked Language Modeling?

- **作者 / 发表**：Alexander Wettig, Tianyu Gao, Zexuan Zhong, Danqi Chen；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.eacl-main.217)
- **领域定位**：Probing, Representation, Hidden, LLM, Behavior, Analyze
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Probing、Representation、Hidden、LLM、Behavior 展开；在不同层或隐状态上训练探针，并比较层间可读性、跨任务稳定性或表征相似性。
- **摘要概括**：摘要所强调的核心线索包括 masked, mlms, conventionally, mask, tokens, belief；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 64. Large Language Models Are Reasoning Teachers

- **作者 / 发表**：Namgyu Ho, Laura Schmid, Se-Young Yun；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.acl-long.830)
- **领域定位**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 recent, works, shown, chain-of-thought, prompting, elicit；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 65. Evaluating Open-Domain Question Answering in the Era of Large Language Models

- **作者 / 发表**：Ehsan Kamalloo, Nouha Dziri, Charles L. A. Clarke, Davood Rafiei；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.acl-long.307)
- **领域定位**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Evaluation、Reasoning、Hallucination、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 lexical, matching, remains, facto, evaluation, method；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 66. Towards Adaptive Prefix Tuning for Parameter-Efficient Language Model Fine-tuning

- **作者 / 发表**：Zhen-Ru Zhang, Chuanqi Tan, Haiyang Xu, Chengyu Wang, Jun Huang, Songfang Huang；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.acl-short.107)
- **领域定位**：Probing, Representation, Evaluation, Hidden, LLM, Layer, Analyze
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Probing、Representation、Evaluation、Hidden、LLM 展开；在不同层或隐状态上训练探针，并比较层间可读性、跨任务稳定性或表征相似性。
- **摘要概括**：摘要所强调的核心线索包括 fine-tuning, pre-trained, various, downstream, tasks, whole；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 67. Element-aware Summarization with Large Language Models: Expert-aligned Evaluation and Chain-of-Thought Method

- **作者 / 发表**：Yiming Wang, Zhuosheng Zhang, Rui Wang；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.acl-long.482)
- **领域定位**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Evaluation、Reasoning、Hallucination、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 automatic, summarization, generates, concise, summaries, contain；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 68. SCOTT: Self-Consistent Chain-of-Thought Distillation

- **作者 / 发表**：Peifeng Wang, Zhengyang Wang, Zheng Li, Yifan Gao, Bing Yin, Xiang Ren；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.acl-long.304)
- **领域定位**：Rationale, Reasoning, Hallucination, Behavior, Analyze
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Rationale、Reasoning、Hallucination、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 beyond, certain, scale, demonstrate, emergent, capability；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 69. Analyzing Transformers in Embedding Space

- **作者 / 发表**：Guy Dar, Mor Geva, Ankit Gupta, Jonathan Berant；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.acl-long.893)
- **领域定位**：Probing, Representation, Transformer, Layer, Analyze
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Probing、Representation、Transformer、Layer、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 understanding, transformer-based, attracted, significant, attention, heart；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 70. EfficientVLM: Fast and Accurate Vision-Language Models via Knowledge Distillation and Modal-adaptive Pruning

- **作者 / 发表**：Tiannan Wang, Wangchunshu Zhou, Yan Zeng, Xinsong Zhang；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-acl.873)
- **领域定位**：Analysis, MLLM, Hidden, Layer, Control
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Analysis、MLLM、Hidden、Layer、Control 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 pre-trained, vision-language, vlms, achieved, impressive, results；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 8 批：论文 71-80

**本批领域发展流程**：研究进入多语言、提示因果性和 SAE 等方向，解释方法开始关注表示的几何结构与跨语言共享。

### 71. Explaining Data Patterns in Natural Language with Language Models

- **作者 / 发表**：Chandan Singh, John X. Morris, Jyoti Aneja, Alexander M. Rush, Jianfeng Gao；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.blackboxnlp-1.3)
- **领域定位**：Attribution, Rationale, Evaluation, Reasoning, LLM, Behavior, Explain
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Attribution、Rationale、Evaluation、Reasoning、LLM 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, displayed, impressive, ability, harness, natural；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 72. Backpack Language Models

- **作者 / 发表**：John K. Hewitt, John Thickstun, Christopher D. Manning, Percy Liang；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.acl-long.506)
- **领域定位**：Representation, Evaluation, LLM, Behavior, Control
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Representation、Evaluation、LLM、Behavior、Control 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 backpacks, neural, architecture, marries, strong, modeling；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 73. Layered Bias: Interpreting Bias in Pretrained Large Language Models

- **作者 / 发表**：Nirmalendu Prakash, Roy Ka-Wei Lee；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.blackboxnlp-1.22)
- **领域定位**：Evaluation, Lens, Hallucination, LLM, Layer, Evaluate
- **要解决的问题**：解决模型记忆、偏见或社会风险难以从内部状态和输出行为中诊断的问题。
- **怎么解决**：围绕 Evaluation、Lens、Hallucination、LLM、Layer 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, like, palm, excelled, numerous, natural；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 74. Future Lens: Anticipating Subsequent Tokens from a Single Hidden State

- **作者 / 发表**：Koyena Pal, Jiuding Sun, Andrew C. Yuan, Byron Wallace, David Bau；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.conll-1.37)
- **领域定位**：Representation, Lens, MLLM, Hidden, Layer, Evaluate
- **要解决的问题**：解决模型不同层和隐状态中到底编码了哪些语言、知识或行为信息的问题。
- **怎么解决**：围绕 Representation、Lens、MLLM、Hidden、Layer 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 conjecture, hidden, state, vectors, corresponding, individual；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 75. Causal interventions expose implicit situation models for commonsense language understanding

- **作者 / 发表**：Takateru Yamakoshi, James L. McClelland, Adele Ε. Goldberg, Robert D. Hawkins；ACL 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-acl.839)
- **领域定位**：Mechanistic, Transformer, Behavior, Control
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Mechanistic、Transformer、Behavior、Control 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 accounts, human, processing, long, appealed, implicit；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 76. Knowledge Neurons in Pretrained Transformers

- **作者 / 发表**：Damai Dai, Li Dong, Yaru Hao, Zhifang Sui, Baobao Chang, Furu Wei；ACL 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.acl-long.581)
- **领域定位**：Attribution, Hallucination, Hidden, Activation, Control
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Attribution、Hallucination、Hidden、Activation、Control 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 large-scale, pretrained, surprisingly, good, recalling, factual；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 77. Extracting Latent Steering Vectors from Pretrained Language Models

- **作者 / 发表**：Nishant Subramani, Nivedita Suresh, Matthew E. Peters；ACL 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.findings-acl.48)
- **领域定位**：Representation, Steering, Evaluation, Hidden, LLM, Behavior, Control
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Representation、Steering、Evaluation、Hidden、LLM 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 prior, controllable, text, generation, focused, learning；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 78. Metaphors in Pre-Trained Language Models: Probing and Generalization Across Datasets and Languages

- **作者 / 发表**：Ehsan Aghazadeh, Mohsen Fayyaz, Yadollah Yaghoobzadeh；ACL 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.acl-long.144)
- **领域定位**：Probing, Representation, Evaluation, Hidden, LLM, Layer, Detect
- **要解决的问题**：解决模型不同层和隐状态中到底编码了哪些语言、知识或行为信息的问题。
- **怎么解决**：围绕 Probing、Representation、Evaluation、Hidden、LLM 展开；在不同层或隐状态上训练探针，并比较层间可读性、跨任务稳定性或表征相似性。
- **摘要概括**：摘要所强调的核心线索包括 human, languages, full, metaphorical, expressions, metaphors；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 79. What changed? Investigating Debiasing Methods using Causal Mediation Analysis

- **作者 / 发表**：Sullam Jeoung, Jana Diesner；ACL 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.gebnlp-1.26)
- **领域定位**：Analysis, Hidden, LLM, Layer, Detect
- **要解决的问题**：解决模型记忆、偏见或社会风险难以从内部状态和输出行为中诊断的问题。
- **怎么解决**：围绕 Analysis、Hidden、LLM、Layer、Detect 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 previous, examined, debiasing, affect, downstream, tasks；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 80. Can Transformer be Too Compositional? Analysing Idiom Processing in Neural Machine Translation

- **作者 / 发表**：Verna Dankers, Christopher J. Lucas, Ivan Titov；ACL 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.acl-long.252)
- **领域定位**：Representation, Hidden, Behavior, Analyze
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Representation、Hidden、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 unlike, literal, expressions, idioms, meanings, directly；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 9 批：论文 81-90

**本批领域发展流程**：研究把 SAE、注意力和机制分析用于算术、事实性、归因和长链推理，解释目标从表征扩展到可靠性。

### 81. Can Prompt Probe Pretrained Language Models? Understanding the Invisible Risks from a Causal View

- **作者 / 发表**：Boxi Cao, Hongyu Lin, Xianpei Han, Fangchao Liu, Le Sun；ACL 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.acl-long.398)
- **领域定位**：Probing, Evaluation, LLM, Behavior, Analyze
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Probing、Evaluation、LLM、Behavior、Analyze 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 prompt-based, probing, been, widely, used, evaluating；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 82. Domain Knowledge Transferring for Pre-trained Language Model via Calibrated Activation Boundary Distillation

- **作者 / 发表**：Dongha Choi, Hongseok Choi, Hyunju Lee；ACL 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.acl-long.116)
- **领域定位**：Analysis, Hidden, LLM, Activation, Analyze
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Analysis、Hidden、LLM、Activation、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 since, development, wide, pretrained, plms, several；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 83. Analyzing Gender Representation in Multilingual Models

- **作者 / 发表**：Hila Gonen, Shauli Ravfogel, Yoav Goldberg；ACL 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.repl4nlp-1.8)
- **领域定位**：Representation, Rationale, Hidden, LLM, Behavior, Explain
- **要解决的问题**：解决模型记忆、偏见或社会风险难以从内部状态和输出行为中诊断的问题。
- **怎么解决**：围绕 Representation、Rationale、Hidden、LLM、Behavior 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 multilingual, were, shown, allow, nontrivial, transfer；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 84. Towards Faithful Natural Language Explanations: A Study Using Activation Patching in Large Language Models

- **作者 / 发表**：Wei Jie Yeo, Ranjan Satapathy, Erik Cambria；EMNLP 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.emnlp-main.529)
- **领域定位**：Mechanistic, Attribution, Rationale, Hidden, LLM, Activation, Explain
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Mechanistic、Attribution、Rationale、Hidden、LLM 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 llms, capable, generating, persuasive, natural, explanations；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 85. A Causal Lens for Evaluating Faithfulness Metrics

- **作者 / 发表**：Kerem Zaman, Shashank Srivastava；EMNLP 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.emnlp-main.1496)
- **领域定位**：Attribution, Rationale, Evaluation, Reasoning, LLM, Behavior, Control
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Attribution、Rationale、Evaluation、Reasoning、LLM 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 llms, offer, natural, explanations, alternative, feature；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 86. A Survey on Sparse Autoencoders: Interpreting the Internal Mechanisms of Large Language Models

- **作者 / 发表**：Dong Shu, Xuansheng Wu, Haiyan Zhao, Daking Rai, Ziyu Yao, Ninghao Liu, Mengnan Du；EMNLP 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.findings-emnlp.89)
- **领域定位**：SAE, LLM, Behavior, Analyze
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 SAE、LLM、Behavior、Analyze 展开；结合稀疏自编码器或稀疏特征分解，把混杂激活转成可定位的特征。
- **摘要概括**：摘要所强调的核心线索包括 内部表征、行为评测和可靠性；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 87. Route Sparse Autoencoder to Interpret Large Language Models

- **作者 / 发表**：Wei Shi, Sihang Li, Tao Liang, Mingyang Wan, Guojun Ma, Xiang Wang, Xiangnan He；EMNLP 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.emnlp-main.346)
- **领域定位**：SAE, LLM, Behavior, Analyze
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 SAE、LLM、Behavior、Analyze 展开；结合稀疏自编码器或稀疏特征分解，把混杂激活转成可定位的特征。
- **摘要概括**：摘要所强调的核心线索包括 内部表征、行为评测和可靠性；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 88. Interpretability Analysis of Arithmetic In-Context Learning in Large Language Models

- **作者 / 发表**：Gregory Polyakov, Christian Hepting, Carsten Eickhoff, Seyed Ali Bahrainian；EMNLP 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.emnlp-main.92)
- **领域定位**：Analysis, LLM, Behavior, Analyze
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Analysis、LLM、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 analysis；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 89. Logit Space Constrained Fine-Tuning for Mitigating Hallucinations in LLM-Based Recommender Systems

- **作者 / 发表**：Jie Deng, Qingfeng Chen, Debo Cheng, Jiuyong Li, Lin Liu；EMNLP 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.emnlp-main.1491)
- **领域定位**：Representation, Evaluation, Hallucination, Hidden, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Representation、Evaluation、Hallucination、Hidden、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, gained, increasing, attention, recommender, systems；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 90. Back Attention: Understanding and Enhancing Multi-Hop Reasoning in Large Language Models

- **作者 / 发表**：Zeping Yu, Yonatan Belinkov, Sophia Ananiadou；EMNLP 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.emnlp-main.567)
- **领域定位**：Representation, Evaluation, Reasoning, Hidden, Layer, Analyze
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Representation、Evaluation、Reasoning、Hidden、Layer 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, perform, latent, multi-hop, reasoning, prompts；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 10 批：论文 91-100

**本批领域发展流程**：研究集中处理事实性、归因和 faithfulness，强调用因果或内部状态验证自然语言解释是否真的对应模型决策。

### 91. CIKT: A Collaborative and Iterative Knowledge Tracing Framework with Large Language Models

- **作者 / 发表**：R N Li, Shuqun Wu, Jun Wang, Wei Zhang；EMNLP 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.emnlp-main.975)
- **领域定位**：Representation, Evaluation, Hidden, LLM, Behavior, Evaluate
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Representation、Evaluation、Hidden、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 knowledge, tracing, aims, student, learning, state；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 92. Refining Attention for Explainable and Noise-Robust Fact-Checking with Transformers

- **作者 / 发表**：Jean-Flavien Bussotti, Paolo Papotti；EMNLP 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.emnlp-main.1295)
- **领域定位**：Evaluation, Transformer, Behavior, Detect
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Evaluation、Transformer、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 tasks, like, question, answering, factchecking, must；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 93. A Comprehensive Survey on the Trustworthiness of Large Language Models in Healthcare

- **作者 / 发表**：Maha Aljohani, Jun Hou, Sindhura Kommu, Xuan Wang；EMNLP 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.findings-emnlp.356)
- **领域定位**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Evaluation、Reasoning、Hallucination、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 application, llms, healthcare, holds, significant, promise；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 94. Large Language Models Meet Knowledge Graphs for Question Answering: Synthesis and Opportunities

- **作者 / 发表**：Chuangtao Ma, Yongrui Chen, Tianxing Wu, Arijit Khan, Haofen Wang；EMNLP 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.emnlp-main.1249)
- **领域定位**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Evaluation、Reasoning、Hallucination、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, demonstrated, remarkable, performance, questionanswering, tasks；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 95. LLMs as a synthesis between symbolic and distributed approaches to language

- **作者 / 发表**：Gemma Boleda；EMNLP 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.findings-emnlp.498)
- **领域定位**：Representation, Reasoning, Hidden, Behavior, Analyze
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Representation、Reasoning、Hidden、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 since, middle, century, fierce, battle, being；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 96. Mechanistic Understanding and Mitigation of Language Model Non-Factual Hallucinations

- **作者 / 发表**：Lei Yu, Meng Cao, Jackie CK Cheung, Yue Dong；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-emnlp.466)
- **领域定位**：Mechanistic, Representation, Evaluation, Hallucination, Hidden, Layer, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Mechanistic、Representation、Evaluation、Hallucination、Hidden 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 state-of-the-art, sometimes, generate, non-factual, hallucinations, misalign；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 97. Unlocking the Future: Exploring Look-Ahead Planning Mechanistic Interpretability in Large Language Models

- **作者 / 发表**：Tianyi Men, Pengfei Cao, Zhuoran Jin, Yubo Chen, Kang Liu, Jun Zhao；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.440)
- **领域定位**：Mechanistic, LLM, Behavior, Analyze
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Mechanistic、LLM、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 mechanistic；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 98. Making Reasoning Matter: Measuring and Improving Faithfulness of Chain-of-Thought Reasoning

- **作者 / 发表**：Debjit Paul, Robert West, Antoine Bosselut, Boi Faltings；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-emnlp.882)
- **领域定位**：Rationale, Reasoning, Hallucination, Behavior, Analyze
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Rationale、Reasoning、Hallucination、Behavior、Analyze 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 llms, been, shown, perform, better, when；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 99. Model Internals-based Answer Attribution for Trustworthy Retrieval-Augmented Generation

- **作者 / 发表**：Jirui Qi, Gabriele Sarti, Raquel Fernández, Arianna Bisazza；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.347)
- **领域定位**：Attribution, Rationale, Evaluation, LLM, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Attribution、Rationale、Evaluation、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 ensuring, verifiability, answers, fundamental, challenge, retrieval-augmented；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 100. Encourage or Inhibit Monosemanticity? Revisit Monosemanticity from a Feature Decorrelation Perspective

- **作者 / 发表**：Hanqi Yan, Yanzheng Xiang, Guangyi Chen, Yifei Wang, Lin Gui, Yulan He；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.582)
- **领域定位**：Probing, Representation, SAE, Hidden, LLM, Behavior, Analyze
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Probing、Representation、SAE、Hidden、LLM 展开；在不同层或隐状态上训练探针，并比较层间可读性、跨任务稳定性或表征相似性。
- **摘要概括**：摘要所强调的核心线索包括 better, interpret, intrinsic, mechanism, llms, recent；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 11 批：论文 101-110

**本批领域发展流程**：研究把解释性推进到故事摘要、概念解释、知识更新和多模态神经元，关注解释的可读性与可验证性。

### 101. FaithScore: Fine-grained Evaluations of Hallucinations in Large Vision-Language Models

- **作者 / 发表**：Liqiang Jing, Ruosen Li, Yunmo Chen, Xinya Du；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-emnlp.290)
- **领域定位**：Rationale, Evaluation, MLLM, Hallucination, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Rationale、Evaluation、MLLM、Hallucination、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 introduce, faithscore, faithfulness, atomic, image, facts；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 102. Lookback Lens: Detecting and Mitigating Contextual Hallucinations in Large Language Models Using Only Attention Maps

- **作者 / 发表**：Yung-Sung Chuang, Linlu Qiu, Cheng-Yu Hsieh, Ranjay Krishna, Yoon Kim, James Glass；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.84)
- **领域定位**：Probing, Representation, Lens, Hallucination, Hidden, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Probing、Representation、Lens、Hallucination、Hidden 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 when, asked, summarize, articles, answer, questions；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 103. Neuron-Level Knowledge Attribution in Large Language Models

- **作者 / 发表**：Zeping Yu, Sophia Ananiadou；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.191)
- **领域定位**：Attribution, Hidden, LLM, Layer, Control
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Attribution、Hidden、LLM、Layer、Control 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 identifying, important, neurons, final, predictions, essential；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 104. Unveiling Factual Recall Behaviors of Large Language Models through Knowledge Neurons

- **作者 / 发表**：Yifei Wang, Yuheng Chen, Wanting Wen, Yu Sheng, Linjing Li, Daniel Dajun Zeng；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.420)
- **领域定位**：Analysis, Hallucination, Hidden, Behavior, Analyze
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Analysis、Hallucination、Hidden、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 analysis；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 105. Activation Scaling for Steering and Interpreting Language Models

- **作者 / 发表**：Niklas Stoehr, Kevin Du, Vésteinn Snæbjarnarson, Robert West, Ryan Cotterell, Aaron Schein；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-emnlp.479)
- **领域定位**：Attribution, Rationale, Steering, Hidden, LLM, Activation, Control
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Attribution、Rationale、Steering、Hidden、LLM 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 given, prompt, rome, steer, flip, prediction；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 106. STORYSUMM: Evaluating Faithfulness in Story Summarization

- **作者 / 发表**：Melanie Subbiah, Faisal Ladhak, Akankshya Mishra, Griffin Adams, Lydia B. Chilton, Kathleen McKeown；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.557)
- **领域定位**：Rationale, Evaluation, LLM, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Rationale、Evaluation、LLM、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 human, evaluation, been, gold, standard, checking；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 107. Evaluating Readability and Faithfulness of Concept-based Explanations

- **作者 / 发表**：Meng Li, Haoran Jin, Ruixuan Huang, Zhihao Xu, Defu Lian, Zijia Lin, Di Zhang, Xiting Wang；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.36)
- **领域定位**：Representation, Rationale, Evaluation, Hidden, LLM, Behavior, Explain
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Representation、Rationale、Evaluation、Hidden、LLM 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 growing, popularity, general-purpose, llms, comes, need；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 108. Does Fine-Tuning LLMs on New Knowledge Encourage Hallucinations?

- **作者 / 发表**：Zorik Gekhman, Gal Yona, Roee Aharoni, Matan Eyal, Amir Feder, Roi Reichart, Jonathan Herzig；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.444)
- **领域定位**：Analysis, Hallucination, LLM, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Analysis、Hallucination、LLM、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 when, aligned, supervised, fine-tuning, encounter, factual；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 109. A Comprehensive Survey of Hallucination in Large Language, Image, Video and Audio Foundation Models

- **作者 / 发表**：Pranab Sahoo, Prabhash Meharia, Akash Ghosh, Sriparna Saha, Vinija Jain, Aman Chadha；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-emnlp.685)
- **领域定位**：Analysis, MLLM, Hallucination, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Analysis、MLLM、Hallucination、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 analysis；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 110. Null-Shot Prompting: Rethinking Prompting Large Language Models With Hallucination

- **作者 / 发表**：Pittawat Taveekitworachai, Febri Abdullah, Ruck Thawonmas；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.740)
- **领域定位**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Evaluation、Reasoning、Hallucination、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 investigates, interesting, phenomenon, where, observe, performance；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 12 批：论文 111-120

**本批领域发展流程**：研究围绕自然语言解释、长上下文失败、未知问题和领域概念，形成解释生成与解释评估并行的路线。

### 111. Understanding Faithfulness and Reasoning of Large Language Models on Plain Biomedical Summaries

- **作者 / 发表**：Biaoyan Fang, Xiang Dai, Sarvnaz Karimi；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-emnlp.578)
- **领域定位**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 generating, plain, biomedical, summaries, llms, enhance；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 112. Advancing Large Language Model Attribution through Self-Improving

- **作者 / 发表**：Lei Huang, Xiaocheng Feng, Weitao Ma, Liang Zhao, Yuchun Fan, Weihong Zhong, Dan Xu, Qing Yang, et al.；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.223)
- **领域定位**：Attribution, LLM, Behavior, Analyze
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Attribution、LLM、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 huang, xiaocheng, feng, weitao, liang, zhao；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 113. Backward Lens: Projecting Language Model Gradients into the Vocabulary Space

- **作者 / 发表**：Shahar Katz, Yonatan Belinkov, Mor Geva, Lior Wolf；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.142)
- **领域定位**：Attribution, Representation, Lens, Hidden, LLM, Behavior, Analyze
- **要解决的问题**：解决模型不同层和隐状态中到底编码了哪些语言、知识或行为信息的问题。
- **怎么解决**：围绕 Attribution、Representation、Lens、Hidden、LLM 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 understanding, transformer-based, learn, recall, information, goal；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 114. Towards Interpretable Sequence Continuation: Analyzing Shared Circuits in Large Language Models

- **作者 / 发表**：Michael Lan, Philip H. S. Torr, Fazl Barez；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.699)
- **领域定位**：Mechanistic, Representation, Evaluation, Hidden, LLM, Behavior, Detect
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Mechanistic、Representation、Evaluation、Hidden、LLM 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 while, transformer, exhibit, strong, capabilities, linguistic；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 115. XRec: Large Language Models for Explainable Recommendation

- **作者 / 发表**：Qiyao Ma, Xubin Ren, Chao Huang；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-emnlp.22)
- **领域定位**：Representation, Rationale, Hidden, LLM, Behavior, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Representation、Rationale、Hidden、LLM、Behavior 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 recommender, systems, help, users, navigate, information；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 116. APPLS: Evaluating Evaluation Metrics for Plain Language Summarization

- **作者 / 发表**：Yue Guo, Tal August, Gondy Leroy, Trevor Cohen, Lucy Lu Wang；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.519)
- **领域定位**：Rationale, Evaluation, LLM, Behavior, Detect
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Rationale、Evaluation、LLM、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 while, there, been, significant, development, plain；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 117. XplainLLM: A Knowledge-Augmented Dataset for Reliable Grounded Explanations in LLMs

- **作者 / 发表**：Zichen Chen, Jianda Chen, Ambuj K. Singh, Misha Sra；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.432)
- **领域定位**：Rationale, Evaluation, Reasoning, Hallucination, Behavior, Detect
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、Hallucination、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, achieved, remarkable, success, natural, tasks；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 118. MMNeuron: Discovering Neuron-Level Domain-Specific Interpretation in Multimodal Large Language Model

- **作者 / 发表**：Jiahao Huo, Yibo Yan, Boren Hu, Yutao Yue, Xuming Hu；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.387)
- **领域定位**：Representation, Lens, MLLM, Hidden, Behavior, Analyze
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Representation、Lens、MLLM、Hidden、Behavior 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 projecting, visual, features, word, embedding, space；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 119. Interpreting Context Look-ups in Transformers: Investigating Attention-MLP Interactions

- **作者 / 发表**：Clement Neo, Shay B. Cohen, Fazl Barez；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.930)
- **领域定位**：Rationale, Hidden, LLM, Layer, Evaluate
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Rationale、Hidden、LLM、Layer、Evaluate 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 understanding, inner, workings, llms, crucial, advancing；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 120. Chain-of-Note: Enhancing Robustness in Retrieval-Augmented Language Models

- **作者 / 发表**：Wenhao Yu, Hongming Zhang, Xiaoman Pan, Peixin Cao, Kaixin Ma, Jian Li, Hongwei Wang, Dong Yu；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.813)
- **领域定位**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Evaluation、Reasoning、Hallucination、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 retrieval-augmented, ralm, represents, significant, advancement, mitigating；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 13 批：论文 121-130

**本批领域发展流程**：研究从幻觉定义与基准扩展到归因、偏差、心理健康和可控生成，开始把解释用于风险诊断。

### 121. Interpretable Preferences via Multi-Objective Reward Modeling and Mixture-of-Experts

- **作者 / 发表**：Haoxiang Wang, Wei Xiong, Tengyang Xie, Han Zhao, Tong Zhang；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-emnlp.620)
- **领域定位**：Evaluation, LLM, Behavior, Detect
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Evaluation、LLM、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 reinforcement, learning, human, feedback, rlhf, emerged；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 122. Verification and Refinement of Natural Language Explanations through LLM-Symbolic Theorem Proving

- **作者 / 发表**：Xin Quan, Marco Valentino, Louise A. Dennis, André Freitas；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.172)
- **领域定位**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 natural, explanations, represent, proxy, evaluating, explanation-based；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 123. Insights into LLM Long-Context Failures: When Transformers Know but Don’t Tell

- **作者 / 发表**：Muhan Gao, Taiming Lu, Kuai Yu, Adam Byerly, Daniel Khashabi；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-emnlp.447)
- **领域定位**：Probing, Representation, Reasoning, Hidden, Layer, Analyze
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Probing、Representation、Reasoning、Hidden、Layer 展开；在不同层或隐状态上训练探针，并比较层间可读性、跨任务稳定性或表征相似性。
- **摘要概括**：摘要所强调的核心线索包括 llms, exhibit, positional, bias, struggling, utilize；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 124. Don’t Just Say “I don’t know”! Self-aligning Large Language Models for Responding to Unknown Questions with Explanations

- **作者 / 发表**：Yang Deng, Yong Zhao, Moxin Li, See-Kiong Ng, Tat-Seng Chua；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.757)
- **领域定位**：Rationale, Evaluation, LLM, Behavior, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、Evaluation、LLM、Behavior、Explain 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 despite, remarkable, abilities, llms, answer, questions；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 125. Evaluating LLMs for Targeted Concept Simplification for Domain-Specific Texts

- **作者 / 发表**：Sumit Asthana, Hannah Rashkin, Elizabeth Clark, Fantine Huot, Mirella Lapata；EMNLP 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.emnlp-main.357)
- **领域定位**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 useful, application, support, people, reading, complex；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 126. A Mechanistic Interpretation of Arithmetic Reasoning in Language Models using Causal Mediation Analysis

- **作者 / 发表**：Alessandro Stolfo, Yonatan Belinkov, Mrinmaya Sachan；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.435)
- **领域定位**：Mechanistic, Reasoning, Hallucination, Layer, Analyze
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Mechanistic、Reasoning、Hallucination、Layer、Analyze 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 mathematical, reasoning, garnered, significant, attention, recent；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 127. Evaluating Object Hallucination in Large Vision-Language Models

- **作者 / 发表**：Yifan Li, Yifan Du, Kun Zhou, Jinpeng Wang, Zhao Xin, Ji-Rong Wen；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.20)
- **领域定位**：Evaluation, MLLM, Hallucination, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Evaluation、MLLM、Hallucination、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 inspired, superior, abilities, vision-language, lvlm, been；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 128. HaluEval: A Large-Scale Hallucination Evaluation Benchmark for Large Language Models

- **作者 / 发表**：Junyi Li, Xiaoxue Cheng, Xin Zhao, Jian‐Yun Nie, Ji-Rong Wen；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.397)
- **领域定位**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Evaluation、Reasoning、Hallucination、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, chatgpt, prone, generate, hallucinations, content；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 129. Towards Mitigating LLM Hallucination via Self Reflection

- **作者 / 发表**：Ziwei Ji, Tiezheng Yu, Yan Xu, Nayeon Lee, Etsuko Ishii, Pascale Fung；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-emnlp.123)
- **领域定位**：Evaluation, Hallucination, LLM, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Evaluation、Hallucination、LLM、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, shown, promise, generative, knowledge-intensive, tasks；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 130. Sources of Hallucination by Large Language Models on Inference Tasks

- **作者 / 发表**：Nick McKenna, Tianyi Li, Liang Cheng, Mohammad Javad Hosseini, Mark Johnson, Mark Steedman；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-emnlp.182)
- **领域定位**：Probing, Evaluation, Hallucination, LLM, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Probing、Evaluation、Hallucination、LLM、Behavior 展开；在不同层或隐状态上训练探针，并比较层间可读性、跨任务稳定性或表征相似性。
- **摘要概括**：摘要所强调的核心线索包括 llms, claimed, capable, natural, inference, necessary；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 14 批：论文 131-140

**本批领域发展流程**：研究把引用、偏差、风格和推理联系起来，探索模型自我改进、引用生成与可解释决策。

### 131. The Troubling Emergence of Hallucination in Large Language Models - An Extensive Definition, Quantification, and Prescriptive Remediations

- **作者 / 发表**：Vipula Rawte, Swagata Chakraborty, Agnibh Pathak, Anubhav Sarkar, S. M Towhidul Islam Tonmoy, Aman Chadha, Amit Sheth, Amitava Das；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.155)
- **领域定位**：Analysis, Hallucination, LLM, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Analysis、Hallucination、LLM、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 vipula, rawte, swagata, chakraborty, agnibh, pathak；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 132. Automatic Evaluation of Attribution by Large Language Models

- **作者 / 发表**：Xiang Yue, Boshi Wang, Ziru Chen, Kai Zhang, Yu Su, Huan Sun；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-emnlp.307)
- **领域定位**：Attribution, Evaluation, LLM, Behavior, Evaluate
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Attribution、Evaluation、LLM、Behavior、Evaluate 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 recent, focus, development, exemplified, generative, search；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 133. Probing the “Creativity” of Large Language Models: Can models produce divergent semantic association?

- **作者 / 发表**：Honghua Chen, Nai Ding；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-emnlp.858)
- **领域定位**：Probing, LLM, Behavior, Analyze
- **要解决的问题**：解决模型不同层和隐状态中到底编码了哪些语言、知识或行为信息的问题。
- **怎么解决**：围绕 Probing、LLM、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 possess, remarkable, capacity, processing, remains, unclear；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 134. Adversarial Robustness for Large Language NER models using Disentanglement and Word Attributions

- **作者 / 发表**：Xiaomeng Jin, Bhanukiran Vinzamuri, Sriram Venkatapathy, Heng Ji, Pradeep Natarajan；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-emnlp.830)
- **领域定位**：Attribution, Representation, Evaluation, LLM, Behavior, Analyze
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Attribution、Representation、Evaluation、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 been, widely, used, several, applications, question；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 135. Leveraging Structured Information for Explainable Multi-hop Question Answering and Reasoning

- **作者 / 发表**：Ruosen Li, Xinya Du；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-emnlp.452)
- **领域定位**：Attribution, Rationale, Evaluation, Reasoning, Hallucination, Behavior, Detect
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Attribution、Rationale、Evaluation、Reasoning、Hallucination 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 neural, including, llms, achieve, superior, performance；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 136. Towards Interpretable Mental Health Analysis with Large Language Models

- **作者 / 发表**：Kailai Yang, Shaoxiong Ji, Tianlin Zhang, Qianqian Xie, Ziyan Kuang, Sophia Ananiadou；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.370)
- **领域定位**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 latest, llms, chatgpt, exhibit, strong, capabilities；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 137. Rethinking the Evaluation for Conversational Recommendation in the Era of Large Language Models

- **作者 / 发表**：Xiaolei Wang, Xinyu Tang, Xin Zhao, Jingyuan Wang, Ji-Rong Wen；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.621)
- **领域定位**：Rationale, Evaluation, LLM, Behavior, Explain
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Rationale、Evaluation、LLM、Behavior、Explain 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 recent, success, llms, shown, great, potential；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 138. Explainable Claim Verification via Knowledge-Grounded Reasoning with Large Language Models

- **作者 / 发表**：Haoran Wang, Kai Shu；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-emnlp.416)
- **领域定位**：Rationale, Evaluation, Reasoning, LLM, Behavior, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 claim, verification, plays, crucial, role, combating；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 139. Distilling ChatGPT for Explainable Automated Student Answer Assessment

- **作者 / 发表**：Jiazheng Li, Lin Gui, Yuxiang Zhou, David West, Cesare Aloisi, Yulan He；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-emnlp.399)
- **领域定位**：Rationale, Evaluation, LLM, Behavior, Evaluate
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、Evaluation、LLM、Behavior、Evaluate 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 providing, explainable, faithful, feedback, crucial, automated；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 140. Self-Detoxifying Language Models via Toxification Reversal

- **作者 / 发表**：Chak Tou Leong, Yi Cheng, Jiashuo Wang, Jian Wang, Wenjie Li；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.269)
- **领域定位**：Representation, Steering, Hidden, LLM, Layer, Control
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Representation、Steering、Hidden、LLM、Layer 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 detoxification, aims, minimize, risk, generating, offensive；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 15 批：论文 141-150

**本批领域发展流程**：研究覆盖视觉语言稀疏专家、链式思考、隐私神经元和临床特征抽取，解释性逐步进入应用系统。

### 141. The Curious Case of Hallucinatory (Un)answerability: Finding Truths in the Hidden States of Over-Confident Large Language Models

- **作者 / 发表**：Aviv Slobodkin, Omer Goldman, Avi Caciularu, Ido Dagan, Shauli Ravfogel；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.220)
- **领域定位**：Representation, Rationale, Hallucination, Hidden, Behavior, Analyze
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Representation、Rationale、Hallucination、Hidden、Behavior 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 llms, been, shown, possess, impressive, capabilities；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 142. Learning Interpretable Style Embeddings via Prompting LLMs

- **作者 / 发表**：Ajay Patel, Delip Rao, Ansh Kothary, Kathleen McKeown, Chris Callison-Burch；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-emnlp.1020)
- **领域定位**：Attribution, Representation, Evaluation, Hidden, LLM, Behavior, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Attribution、Representation、Evaluation、Hidden、LLM 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 style, representation, learning, builds, content-independent, representations；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 143. Disentangling Transformer Language Models as Superposed Topic Models

- **作者 / 发表**：Jia Lim, Hady W. Lauw；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.534)
- **领域定位**：SAE, Hidden, LLM, Behavior, Analyze
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 SAE、Hidden、LLM、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 topic, modelling, established, research, area, where；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 144. Large Language Models Can Self-Improve

- **作者 / 发表**：Jiaxin Huang, Shixiang Gu, Le Hou, Yuexin Wu, Xuezhi Wang, Hongkun Yu, Jiawei Han；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.67)
- **领域定位**：Rationale, Evaluation, Reasoning, LLM, Behavior, Analyze
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, achieved, excellent, performances, various, tasks；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 145. Enabling Large Language Models to Generate Text with Citations

- **作者 / 发表**：Tianyu Gao, H. W. Yen, Jiatong Yu, Danqi Chen；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.398)
- **领域定位**：Evaluation, Hallucination, LLM, Behavior, Detect
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Evaluation、Hallucination、LLM、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, emerged, widely-used, tool, information, seeking；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 146. Document-Level Machine Translation with Large Language Models

- **作者 / 发表**：Longyue Wang, Chenyang Lyu, Tianbo Ji, Zhirui Zhang, Dian Yu, Shuming Shi, Zhaopeng Tu；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.1036)
- **领域定位**：Probing, Evaluation, LLM, Behavior, Evaluate
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Probing、Evaluation、LLM、Behavior、Evaluate 展开；在不同层或隐状态上训练探针，并比较层间可读性、跨任务稳定性或表征相似性。
- **摘要概括**：摘要所强调的核心线索包括 llms, chatgpt, produce, coherent, cohesive, relevant；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 147. “Kelly is a Warm Person, Joseph is a Role Model”: Gender Biases in LLM-Generated Reference Letters

- **作者 / 发表**：Yixin Wan, George Pu, Jiao Sun, Aparna Garimella, Kai-Wei Chang, Nanyun Peng；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-emnlp.243)
- **领域定位**：Evaluation, Hallucination, LLM, Behavior, Detect
- **要解决的问题**：解决模型记忆、偏见或社会风险难以从内部状态和输出行为中诊断的问题。
- **怎么解决**：围绕 Evaluation、Hallucination、LLM、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, recently, emerged, effective, tool, assist；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 148. Extractive Summarization via ChatGPT for Faithful Summary Generation

- **作者 / 发表**：Haopeng Zhang, Xiao Liu, Jiawei Zhang；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-emnlp.214)
- **领域定位**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 extractive, summarization, crucial, task, natural, processing；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 149. Theory of Mind for Multi-Agent Collaboration via Large Language Models

- **作者 / 发表**：Huao Li, Chong Yu, Simon Stepputtis, Joseph Campbell, Dana Hughes, Charles Lewis, Katia Sycara；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.13)
- **领域定位**：Representation, Reasoning, Hallucination, Behavior, Detect
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Representation、Reasoning、Hallucination、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 while, llms, demonstrated, impressive, accomplishments, both；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 150. Automatic Prompt Augmentation and Selection with Chain-of-Thought from Labeled Data

- **作者 / 发表**：Kashun Shum, Shizhe Diao, Tong Zhang；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-emnlp.811)
- **领域定位**：Attribution, Rationale, Evaluation, Reasoning, LLM, Behavior, Analyze
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Attribution、Rationale、Evaluation、Reasoning、LLM 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 chain-of-thought, advances, reasoning, abilities, llms, achieves；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 16 批：论文 151-160

**本批领域发展流程**：研究重新审视输入显著性、结构 probing、对比解释和解释正则化，重点转向解释忠实度的实验协议。

### 151. Scaling Vision-Language Models with Sparse Mixture of Experts

- **作者 / 发表**：Sheng Shen, Zhewei Yao, Chunyuan Li, Trevor Darrell, Kurt Keutzer, Yuxiong He；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-emnlp.758)
- **领域定位**：Evaluation, MLLM, LLM, Behavior, Evaluate
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Evaluation、MLLM、LLM、Behavior、Evaluate 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 field, natural, processing, made, significant, strides；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 152. The CoT Collection: Improving Zero-shot and Few-shot Learning of Language Models via Chain-of-Thought Fine-Tuning

- **作者 / 发表**：Seungone Kim, Segyeong Joo, Doyoung Kim, Joel Jang, Seonghyeon Ye, Jamin Shin, Minjoon Seo；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.782)
- **领域定位**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 less, parameters, known, perform, poorly, chain-of-thought；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 153. Answering Questions by Meta-Reasoning over Multiple Chains of Thought

- **作者 / 发表**：Ori Yoran, Tomer Wolfson, Ben Bogin, U. Katz, Daniel Deutch, Jonathan Berant；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.364)
- **领域定位**：Rationale, Evaluation, Reasoning, LLM, Behavior, Explain
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 modern, systems, multi-hop, question, answering, typically；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 154. DEPN: Detecting and Editing Privacy Neurons in Pretrained Language Models

- **作者 / 发表**：Xinwei Wu, Junzhuo Li, Minghui Xu, Weilong Dong, Shuangzhi Wu, Chao Bian, Deyi Xiong；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.174)
- **领域定位**：Attribution, Hidden, LLM, Activation, Detect
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Attribution、Hidden、LLM、Activation、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 pretrained, learned, vast, amount, human, knowledge；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 155. CHiLL: Zero-shot Custom Interpretable Feature Extraction from Clinical Notes with Large Language Models

- **作者 / 发表**：Denis McInerney, Geoffrey S. Young, Jan-Willem van de Meent, Byron Wallace；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-emnlp.568)
- **领域定位**：Probing, LLM, Behavior, Evaluate
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Probing、LLM、Behavior、Evaluate 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 chill, prompts, llms, expert-crafted, queries, generate；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 156. LogiCoT: Logical Chain-of-Thought Instruction Tuning

- **作者 / 发表**：Hanmeng Liu, Zhiyang Teng, Leyang Cui, Chaoli Zhang, Qiji Zhou, Yue Zhang；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.findings-emnlp.191)
- **领域定位**：Rationale, Evaluation, Reasoning, Behavior, Analyze
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、Behavior、Analyze 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 generative, pre-trained, transformer, gpt-, demonstrates, impressive；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 157. Do Transformers Parse while Predicting the Masked Word?

- **作者 / 发表**：Haoyu Zhao, Abhishek Panigrahi, Rong Ge, Sanjeev Arora；EMNLP 2023/01
- **论文链接**：[正式页面](https://aclanthology.org/2023.emnlp-main.1029)
- **领域定位**：Probing, Representation, LLM, Behavior, Analyze
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Probing、Representation、LLM、Behavior、Analyze 展开；在不同层或隐状态上训练探针，并比较层间可读性、跨任务稳定性或表征相似性。
- **摘要概括**：摘要所强调的核心线索包括 pre-trained, been, shown, encode, linguistic, structures；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 158. “Will You Find These Shortcuts?” A Protocol for Evaluating the Faithfulness of Input Salience Methods for Text Classification

- **作者 / 发表**：Jasmijn Bastings, Sebastian Ebert, Polina Zablotskaia, Anders Sandholm, Katja Filippova；EMNLP 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.emnlp-main.64)
- **领域定位**：Attribution, Rationale, Evaluation, Transformer, Behavior, Analyze
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Attribution、Rationale、Evaluation、Transformer、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 feature, attribution, input, salience, methods, assign；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 159. Probing for Constituency Structure in Neural Language Models

- **作者 / 发表**：David Arps, Younes Samih, Laura Kallmeyer, Hassan Sajjad；EMNLP 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.findings-emnlp.502)
- **领域定位**：Probing, Representation, Hidden, LLM, Activation, Analyze
- **要解决的问题**：解决模型不同层和隐状态中到底编码了哪些语言、知识或行为信息的问题。
- **怎么解决**：围绕 Probing、Representation、Hidden、LLM、Activation 展开；在不同层或隐状态上训练探针，并比较层间可读性、跨任务稳定性或表征相似性。
- **摘要概括**：摘要所强调的核心线索包括 extent, contextual, neural, implicitly, learn, syntactic；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 160. Interpreting Language Models with Contrastive Explanations

- **作者 / 发表**：Kayo Yin, Graham Neubig；EMNLP 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.emnlp-main.14)
- **领域定位**：Rationale, LLM, Behavior, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、LLM、Behavior、Explain 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 interpretability, methods, often, used, explain, decisions；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 17 批：论文 161-170

**本批领域发展流程**：研究从知识校准和训练数据追踪延伸到技能神经元，开始把可解释单元与知识来源、能力和编辑联系起来。

### 161. ER-Test: Evaluating Explanation Regularization Methods for Language Models

- **作者 / 发表**：Brihi Joshi, Aaron Chan, Ziyi Liu, Shaoliang Nie, Maziar Sanjabi, Hamed Firooz, Xiang Ren；EMNLP 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.findings-emnlp.242)
- **领域定位**：Rationale, Evaluation, LLM, Behavior, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、Evaluation、LLM、Behavior、Explain 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 explaining, humans, would, solve, given, task；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 162. Can language models learn from explanations in context?

- **作者 / 发表**：Andrew K. Lampinen, Ishita Dasgupta, Stephanie C. Y. Chan, Kory W. Mathewson, Mh Tessler, Antonia Creswell, James L. McClelland, Jane Wang, et al.；EMNLP 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.findings-emnlp.38)
- **领域定位**：Rationale, LLM, Behavior, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、LLM、Behavior、Explain 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 andrew, lampinen, ishita, dasgupta, stephanie, chan；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 163. KOLD: Korean Offensive Language Dataset

- **作者 / 发表**：Younghoon Jeong, Juhyun Oh, Jongwon Lee, Jaimeen Ahn, Jihyung Moon, Sungjoon Park, Alice Oh；EMNLP 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.emnlp-main.744)
- **领域定位**：Evaluation, Transformer, Behavior, Detect
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Evaluation、Transformer、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 recent, directions, offensive, detection, hierarchical, modeling；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 164. FLUTE: Figurative Language Understanding through Textual Explanations

- **作者 / 发表**：Tuhin Chakrabarty, Arkadiy Saakyan, Debanjan Ghosh, Smaranda Muresan；EMNLP 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.emnlp-main.481)
- **领域定位**：Probing, Rationale, Evaluation, Reasoning, LLM, Behavior, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Probing、Rationale、Evaluation、Reasoning、LLM 展开；在不同层或隐状态上训练探针，并比较层间可读性、跨任务稳定性或表征相似性。
- **摘要概括**：摘要所强调的核心线索包括 figurative, understanding, been, recently, framed, recognizing；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 165. Calibrating Factual Knowledge in Pretrained Language Models

- **作者 / 发表**：Qingxiu Dong, Damai Dai, Yifan Song, Jingjing Xu, Zhifang Sui, Lei Li；EMNLP 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.findings-emnlp.438)
- **领域定位**：Probing, MLLM, Hallucination, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Probing、MLLM、Hallucination、Behavior、Detect 展开；在不同层或隐状态上训练探针，并比较层间可读性、跨任务稳定性或表征相似性。
- **摘要概括**：摘要所强调的核心线索包括 previous, literature, proved, pretrained, plms, store；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 166. Towards Tracing Knowledge in Language Models Back to the Training Data

- **作者 / 发表**：Ekin Akyürek, Tolga Bolukbasi, Frederick Liu, Binbin Xiong, Ian Tenney, Jacob Andreas, Kelvin Guu；EMNLP 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.findings-emnlp.180)
- **领域定位**：Attribution, Representation, Evaluation, Hallucination, LLM, Behavior, Evaluate
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Attribution、Representation、Evaluation、Hallucination、LLM 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 been, shown, memorize, great, deal, factual；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 167. Finding Skill Neurons in Pre-trained Transformer-based Language Models

- **作者 / 发表**：Xiaozhi Wang, Kaiyue Wen, Zhengyan Zhang, Lei Hou, Zhiyuan Liu, Juanzi Li；EMNLP 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.emnlp-main.765)
- **领域定位**：Analysis, Hidden, LLM, Activation, Analyze
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Analysis、Hidden、LLM、Activation、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 transformer-based, pre-trained, demonstrated, superior, performance, various；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 168. On A Scale From 1 to 5: Quantifying Hallucination in Faithfulness Evaluation

- **作者 / 发表**：Jing Xu, Srinivas Billa, Danny Godbout；NAACL 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.findings-naacl.433)
- **领域定位**：Rationale, Evaluation, Hallucination, LLM, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Hallucination、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 hallucination, been, popular, topic, natural, generation；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 169. Mechanistic Unveiling of Transformer Circuits: Self-Influence as a Key to Model Reasoning

- **作者 / 发表**：Lin Zhang, Li Hu, Di Wang；NAACL 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.findings-naacl.76)
- **领域定位**：Mechanistic, Reasoning, LLM, Behavior, Evaluate
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Mechanistic、Reasoning、LLM、Behavior、Evaluate 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 transformer-based, achieved, significant, success, however, internal；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 170. Rationale Behind Essay Scores: Enhancing S-LLM’s Multi-Trait Essay Scoring with Rationale Generated by LLMs

- **作者 / 发表**：SeongYeub Chu, J. Kim, Bryan Wong, Mun Yong Yi；NAACL 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.findings-naacl.322)
- **领域定位**：Rationale, Evaluation, LLM, Behavior, Evaluate
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、Evaluation、LLM、Behavior、Evaluate 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 existing, automated, essay, scoring, solely, relied；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 18 批：论文 171-180

**本批领域发展流程**：研究进入大规模综述、机制电路、记忆归因、推荐解释和结构化理由阶段，解释性成为可信 NLP 的基础组件。

### 171. On Behalf of the Stakeholders: Trends in NLP Model Interpretability in the Era of LLMs

- **作者 / 发表**：Nitay Calderon, Roi Reichart；NAACL 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.naacl-long.29)
- **领域定位**：Analysis, LLM, Behavior, Analyze
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Analysis、LLM、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 nitay, calderon, reichart, proceedings, conference, nations；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 172. Analyzing Memorization in Large Language Models through the Lens of Model Attribution

- **作者 / 发表**：Tarun Ram Menta, Susmit Agrawal, Chirag Agarwal；NAACL 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.naacl-long.535)
- **领域定位**：Attribution, Lens, LLM, Behavior, Analyze
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Attribution、Lens、LLM、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 tarun, menta, susmit, agrawal, chirag, agarwal；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 173. ReasoningRec: Bridging Personalized Recommendations and Human-Interpretable Explanations through LLM Reasoning

- **作者 / 发表**：Millennium Bismay, Xiangjue Dong, James Caverlee；NAACL 2025/01
- **论文链接**：[正式页面](https://aclanthology.org/2025.findings-naacl.454)
- **领域定位**：Rationale, Evaluation, Reasoning, LLM, Behavior, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 presents, reasoningrec, reasoningbased, recommendation, framework, leverages；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 174. Comparing Explanation Faithfulness between Multilingual and Monolingual Fine-tuned Language Models

- **作者 / 发表**：Zhixue Zhao, Νικόλαος Αλέτρας；NAACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.naacl-long.178)
- **领域定位**：Rationale, LLM, Behavior, Explain
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Rationale、LLM、Behavior、Explain 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 zhixue, zhao, nikolaos, aletras, proceedings, conference；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 175. Can Knowledge Graphs Reduce Hallucinations in LLMs? : A Survey

- **作者 / 发表**：Garima Agrawal, Tharindu Kumarage, Zeyad Alghamdi, Huan Liu；NAACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.naacl-long.219)
- **领域定位**：Analysis, Hallucination, LLM, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Analysis、Hallucination、LLM、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 garima, agrawal, tharindu, kumarage, zeyad, alghamdi；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 176. TofuEval: Evaluating Hallucinations of LLMs on Topic-Focused Dialogue Summarization

- **作者 / 发表**：Liyan Tang, Igor Shalyminov, Amy Wing‐mei Wong, Jon Burnsky, Jake W. Vincent, Yu’an Yang, Siffi Singh, Song Feng, et al.；NAACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.naacl-long.251)
- **领域定位**：Analysis, Hallucination, LLM, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Analysis、Hallucination、LLM、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 liyan, tang, igor, shalyminov, wong, burnsky；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 177. On Large Language Models’ Hallucination with Regard to Known Facts

- **作者 / 发表**：Che Jiang, Biqing Qi, Xiangyu Hong, Dayuan Fu, Yang Cheng, Fandong Meng, Mo Yu, Bowen Zhou, et al.；NAACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.naacl-long.60)
- **领域定位**：Analysis, Hallucination, LLM, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Analysis、Hallucination、LLM、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 jiang, biqing, xiangyu, hong, dayuan, yang；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 178. TriSum: Learning Summarization Ability from Large Language Models with Structured Rationale

- **作者 / 发表**：Pengcheng Jiang, Cao Xiao, Zifeng Wang, Parminder Bhatia, Jimeng Sun, Jiawei Han；NAACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.naacl-long.154)
- **领域定位**：Rationale, LLM, Behavior, Analyze
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、LLM、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 pengcheng, jiang, xiao, zifeng, wang, parminder；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 179. How Interpretable are Reasoning Explanations from Prompting Large Language Models?

- **作者 / 发表**：Yeo Wei Jie, Ranjan Satapathy, Rick Siow Mong Goh, Erik Cambria；NAACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-naacl.138)
- **领域定位**：Rationale, Evaluation, Reasoning, LLM, Behavior, Evaluate
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Reasoning、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 prompt, engineering, garnered, significant, attention, enhancing；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 180. Distilling Text Style Transfer With Self-Explanation From LLMs

- **作者 / 发表**：Chiyu Zhang, Honglong Cai, Yuezhang Li, Yuexin Wu, Le Hou, Muhammad Abdul-Mageed；NAACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.naacl-srw.21)
- **领域定位**：Rationale, LLM, Behavior, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、LLM、Behavior、Explain 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 chiyu, zhang, honglong, yuezhang, yuexin, muhammad；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 19 批：论文 181-190

**本批领域发展流程**：研究结合 Transformer layer lens、concept activation、链式思考和模型编辑，形成从观察、定位到干预的完整闭环。

### 181. DecoderLens: Layerwise Interpretation of Encoder-Decoder Transformers

- **作者 / 发表**：Anna Langedijk, Hosein Mohebbi, Gabriele Sarti, Willem Zuidema, Jaap Jumelet；NAACL 2024/01
- **论文链接**：[正式页面](https://aclanthology.org/2024.findings-naacl.296)
- **领域定位**：Representation, Lens, Reasoning, Hidden, Layer, Analyze
- **要解决的问题**：解决模型不同层和隐状态中到底编码了哪些语言、知识或行为信息的问题。
- **怎么解决**：围绕 Representation、Lens、Reasoning、Hidden、Layer 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 recent, years, several, interpretability, methods, been；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 182. Few-Shot Self-Rationalization with Natural Language Prompts

- **作者 / 发表**：Ana Marasović, Iz Beltagy, Doug Downey, Matthew E. Peters；NAACL 2022/01
- **论文链接**：[正式页面](https://aclanthology.org/2022.findings-naacl.31)
- **领域定位**：Rationale, Evaluation, Transformer, Behavior, Explain
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Rationale、Evaluation、Transformer、Behavior、Explain 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 self-rationalization, predict, task, labels, generate, free-text；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 183. Detecting and Mitigating Hallucination in Large Vision Language Models via Fine-Grained AI Feedback

- **作者 / 发表**：Wenyi Xiao, Ziwei Huang, Leilei Gan, Wanggui He, Haoyuan Li, Zhelun Yu, Fangxun Shu, Hao Jiang, et al.；AAAI 2025/04
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/34744/36899)
- **领域定位**：Evaluation, MLLM, Hallucination, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Evaluation、MLLM、Hallucination、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 rapidly, developing, vision, lvlms, still, face；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 184. Identifying Query-Relevant Neurons in Large Language Models for Long-Form Texts

- **作者 / 发表**：Lihu Chen, Adam Dejl, Francesca Toni；AAAI 2025/04
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/34529/36684)
- **领域定位**：Attribution, Evaluation, Hidden, LLM, Behavior, Detect
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Attribution、Evaluation、Hidden、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, possess, vast, amounts, knowledge, within；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 185. C3oT: Generating Shorter Chain-of-Thought Without Compromising Effectiveness

- **作者 / 发表**：Yu Kang, Xiaorui Sun, Liangyu Chen, Wei Zou；AAAI 2025/04
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/34608/36763)
- **领域定位**：Evaluation, Reasoning, LLM, Behavior, Analyze
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Evaluation、Reasoning、LLM、Behavior、Analyze 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 generating, chain-of-thought, before, deriving, answer, effectively；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 186. Enhancing Chain of Thought Prompting in Large Language Models via Reasoning Patterns

- **作者 / 发表**：Yufeng Zhang, Xuepeng Wang, Lingxiang Wu, Jinqiao Wang；AAAI 2025/04
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/34793/36948)
- **领域定位**：Analysis, Reasoning, LLM, Behavior, Analyze
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Analysis、Reasoning、LLM、Behavior、Analyze 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 chain, thought, prompting, encourage, engage, multi-step；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 187. Controlling Large Language Models Through Concept Activation Vectors

- **作者 / 发表**：Hanyu Zhang, Xiting Wang, Chengao Li, Xiang Ao, He Qin；AAAI 2025/04
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/34778/36933)
- **领域定位**：Steering, Hidden, LLM, Layer, Control
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Steering、Hidden、LLM、Layer、Control 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 llms, widely, deployed, across, various, domains；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 188. Journey to the Center of the Knowledge Neurons: Discoveries of Language-Independent Knowledge Neurons and Degenerate Knowledge Neurons

- **作者 / 发表**：Yuheng Chen, Pengfei Cao, Yubo Chen, Kang K. L. Liu, Jun Zhao；AAAI 2024/03
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/29735/31264)
- **领域定位**：Attribution, Hallucination, Hidden, Behavior, Detect
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Attribution、Hallucination、Hidden、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 pre-trained, plms, contain, vast, amounts, factual；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 189. Mitigating Large Language Model Hallucinations via Autonomous Knowledge Graph-Based Retrofitting

- **作者 / 发表**：Xinyan Guan, Yanjiang Liu, Hongyu Lin, Yaojie Lu, Ben He, Xianpei Han, Le Sun；AAAI 2024/03
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/29770/31326)
- **领域定位**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **要解决的问题**：解决模型输出中的事实性、归因或解释忠实度难以验证的问题。
- **怎么解决**：围绕 Evaluation、Reasoning、Hallucination、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 incorporating, factual, knowledge, graph, regarded, promising；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 190. Attention Guided CAM: Visual Explanations of Vision Transformer Guided by Self-Attention

- **作者 / 发表**：Saebom Leem, Hyunseok Seo；AAAI 2024/03
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/28077/28160)
- **领域定位**：Attribution, Rationale, MLLM, Behavior, Detect
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Attribution、Rationale、MLLM、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 vision, transformer, most, widely, used, computer；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。


## 第 20 批：论文 191-200

**本批领域发展流程**：研究把可解释推荐、视觉推理、学生评估和精确模型编辑纳入统一视野，强调解释对实际决策质量的作用。

### 191. Fine-Tuning Large Language Model Based Explainable Recommendation with Explainable Quality Reward

- **作者 / 发表**：Mengyuan Yang, Mengying Zhu, Yan Wang, Linxun Chen, Yi‐Lei Zhao, Xiuyuan Wang, Bing Han, Xiaolin Zheng, et al.；AAAI 2024/03
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/28777/29489)
- **领域定位**：Rationale, Evaluation, LLM, Behavior, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、Evaluation、LLM、Behavior、Explain 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 model-based, explainable, recommendation, llm-based, systems, provide；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 192. MELO: Enhancing Model Editing with Neuron-Indexed Dynamic LoRA

- **作者 / 发表**：Lang Yu, Qin Chen, Jie Zhou, Liang He；AAAI 2024/03
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/29916/31602)
- **领域定位**：Analysis, Hallucination, Hidden, Behavior, Detect
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Analysis、Hallucination、Hidden、Behavior、Detect 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 llms, shown, great, success, various, natural；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 193. Sparsity-Guided Holistic Explanation for LLMs with Interpretable Inference-Time Intervention

- **作者 / 发表**：Zhen Tan, Tianlong Chen, Zhenyu Zhang, Huan Liu；AAAI 2024/03
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/30160/32058)
- **领域定位**：Rationale, Evaluation, MLLM, LLM, Layer, Explain
- **要解决的问题**：解决模型给出的自然语言理由是否可读、可信并真正反映决策过程的问题。
- **怎么解决**：围绕 Rationale、Evaluation、MLLM、LLM、Layer 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 llms, achieved, unprecedented, breakthroughs, various, natural；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 194. Automated Natural Language Explanation of Deep Visual Neurons with Large Models (Student Abstract)

- **作者 / 发表**：Chenxu Zhao, Wei Qian, Yucheng Shi, Mengdi Huai, Ninghao Liu；AAAI 2024/03
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/30537/32697)
- **领域定位**：Rationale, MLLM, Hidden, Behavior, Explain
- **要解决的问题**：解决模型内部表征分布式、神经元多义以及具体因果机制难以定位的问题。
- **怎么解决**：围绕 Rationale、MLLM、Hidden、Behavior、Explain 展开；通过激活替换、因果干预、反事实比较或局部编辑检验内部信号的作用。
- **摘要概括**：摘要所强调的核心线索包括 interpreting, deep, neural, networks, through, examining；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 195. Bad Actor, Good Advisor: Exploring the Role of Large Language Models in Fake News Detection

- **作者 / 发表**：Beizhe Hu, Qiang Sheng, Juan Cao, Yuhui Shi, Yang Li, Danding Wang, Peng Qi；AAAI 2024/03
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/30214/32159)
- **领域定位**：Rationale, Evaluation, LLM, Behavior, Detect
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Rationale、Evaluation、LLM、Behavior、Detect 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 detecting, fake, news, requires, both, delicate；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 196. A Chain-of-Thought Prompting Approach with LLMs for Evaluating Students’ Formative Assessment Responses in Science

- **作者 / 发表**：Clayton Cohn, Nicole Hutchins, Tuan Anh Le, Gautam Biswas；AAAI 2024/03
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/30364/32416)
- **领域定位**：Rationale, Reasoning, LLM, Behavior, Explain
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Rationale、Reasoning、LLM、Behavior、Explain 展开；结合模型输出、内部状态和对照实验，分析方法对解释质量或模型可靠性的影响。
- **摘要概括**：摘要所强调的核心线索包括 explores, llms, score, explain, short-answer, assessments；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 197. SpikingBERT: Distilling BERT to Train Spiking Language Models Using Implicit Differentiation

- **作者 / 发表**：Malyaban Bal, Abhronil Sengupta；AAAI 2024/03
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/28975/29854)
- **领域定位**：Attribution, Evaluation, Hidden, LLM, Behavior, Evaluate
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Attribution、Evaluation、Hidden、LLM、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, though, growing, exceedingly, powerful, comprises；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 198. T-SciQ: Teaching Multimodal Chain-of-Thought Reasoning via Large Language Model Signals for Science Question Answering

- **作者 / 发表**：Lei Wang, Yi Hu, Jiabang He, Xing Xu, Ning Liu, Hui Liu, Heng Tao Shen；AAAI 2024/03
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/29884/31543)
- **领域定位**：Rationale, Evaluation, MLLM, Reasoning, Behavior, Evaluate
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Rationale、Evaluation、MLLM、Reasoning、Behavior 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 llms, recently, demonstrated, exceptional, performance, various；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 199. Visual Chain-of-Thought Prompting for Knowledge-Based Visual Reasoning

- **作者 / 发表**：Zhenfang Chen, Qinhong Zhou, Yikang Shen, Yining Hong, Zhiqing Sun, Dan Gutfreund, Chuang Gan；AAAI 2024/03
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/27888/27801)
- **领域定位**：Probing, Rationale, Evaluation, MLLM, Reasoning, Behavior, Analyze
- **要解决的问题**：解决模型推理过程不透明、推理链可能不忠实或难以验证的问题。
- **怎么解决**：围绕 Probing、Rationale、Evaluation、MLLM、Reasoning 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 knowledge-based, visual, reasoning, remains, daunting, task；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

### 200. PMET: Precise Model Editing in a Transformer

- **作者 / 发表**：Xiaopeng Li, Shasha Li, Shezheng Song, Jing Yang, Jun Ma, Jie Yu；AAAI 2024/03
- **论文链接**：[正式页面](https://ojs.aaai.org/index.php/AAAI/article/download/29818/31420)
- **领域定位**：Representation, Evaluation, Hallucination, Hidden, Layer, Control
- **要解决的问题**：解决大模型内部机制、输出行为与可解释决策之间缺少稳定对应关系的问题。
- **怎么解决**：围绕 Representation、Evaluation、Hallucination、Hidden、Layer 展开；构造数据集或评测协议，把抽象的解释性问题转化为可复现的行为指标。
- **摘要概括**：摘要所强调的核心线索包括 editing, techniques, modify, minor, proportion, knowledge；总体贡献是把题目中的问题转化为可测量、可比较或可干预的解释性证据。具体结论仍应结合原文实验设置和数据集理解。
- **阅读定位**：这篇论文在当前综述中承担的是从可观察行为/表征分析走向可验证解释或实际应用的一环；使用时应优先核对原文的模型、数据集、指标和干预对照。

## 全部 200 篇的阶段性发展主线

1. **行为与问题定义**：先建立幻觉、偏置、重复、错误推理和解释不忠实等可观察现象。
2. **表征与模块定位**：再用 probing、layer analysis、logit lens、知识神经元、attention/MLP 分析寻找内部承载位置。
3. **因果验证与局部干预**：通过 activation patching、steering、参数编辑、反事实评估和 theorem proving 检查解释是否真正参与决策。
4. **特征分解与结构化解释**：SAE、稀疏特征、共享电路、知识图谱和结构化 rationale 提供更细的解释单元。
5. **可信应用闭环**：解释方法最终用于隐私、RAG 归因、事实性、医疗、推荐、教育、多语言和视觉语言系统。

> 备注：第 1 批为人工精读版；第 2-20 批已按正式论文摘要、综述标签和论文在领域流程中的位置完成逐篇摘要级整理。后续若需要投稿级综述，还应再对重点论文补充定理、实验表格和局限性。
