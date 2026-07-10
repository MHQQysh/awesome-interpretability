# 093. A Comprehensive Survey on the Trustworthiness of Large Language Models in Healthcare

> 逐篇阅读记录：第 93 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Maha Aljohani, Jun Hou, Sindhura Kommu, Xuan Wang
- **发表 venue / date**：EMNLP / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.findings-emnlp.356)
- **领域标签**：Evaluation, Reasoning, Hallucination, Behavior, Detect
- **本地 PDF 文本规模**：约 14,692 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Evaluation为主线，结合论文摘要中的核心设定：The application of large language models (LLMs) in healthcare holds significant promise for enhancing clinical decision-making, medical research, and patient care.However, their integration into real-world clinical setti
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction generate accurate, reliable, and unbiased outputs；12 Privacy Privacy；8 Safety；6 Robustness；4 Robustness；0 Explainability；4 Future Directions privacy-preserving training, and scalable evalua-；2022. PubHealthTab: A public health table-based
- **引言关键线索**：across diverse clinical scenarios while minimizing The application of LLMs in healthcare is advanc- errors, hallucinations, and biases. It also encom- ing rapidly, with the potential to transform clini- passes the model鈥檚 resilience against adversarial cal decision-making, medical research, and patient attacks, ensuring that external manipulations do care. However, incorporating them into healthcare not compromise its integrity. A truly robust LLM systems poses several key challenges that need to in healthcare must demonstrate stability, reliability, be addressed to ensure their reliable and ethical use. and fairness, even when faced with noisy, ambigu- As highlighted in Bi et al. (2024), a major concern ous, or adversarial inputs. Similarly, fairness and is the trustworthiness of AI-enhanced biomedical bias must be addressed to prevent discriminatory 6720 Findings of the Association for
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：worthiness, the trustworthiness of LLMs in inappropriate treatment recommendations. Ensur- healthcare remains underexplored, lacking a ing that generated information is both accurate systematic review that provides a comprehen- and aligned with verified medical knowledge is sive understanding and future insights. This essential. Additionally, privacy concerns arise survey addresses that gap by providing a com- from the risk of exposing sensitive patient data prehensive review of current methodologies during model training and usage, potentially lead- and solutions aimed at mitigating risks across ing to breaches or violations of regulations such key trust dimensions. We analyze how each as HIPAA (Health Insurance Portability and Ac- dimension affects the reliability and ethical de- ployment of healthcare LLMs, synthesize ongo- countability Act) and GDPR (General Data Pro- ing research efforts, and identify critical gaps tection Regulati
- **方法与解释性关系**：该论文主要围绕 `Evaluation, Reasoning, Hallucination, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Table 1, This table provides a, LLMs for healthcare. The, LLMs in medical AI, Table 2, Detailed Comparison of GPT, Models Evaluated for Trust, Healthcare LLMs, Including Model Name
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Table 1: This table provides a structured comparison of datasets used in studies on trust in LLMs for healthcare. The
  - comparison highlights how each dataset contributes to the development of trustworthy LLMs in medical AI.
  - Table 2: Detailed Comparison of GPT Models Evaluated for Trust in Healthcare LLMs, Including Model Name,

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - (LLMs) in healthcare holds significant promise across diverse populations, and ensuring strong
  - tion due to its significant social impact. However, tent of each dataset specifies its composition, while
  - databases), and inference and reasoning (drawing and improved evidence synthesis.
  - conclusions, inferring relationships, and predicting Several benchmarks have emerged to quantify
  - and-effect relationships). module that improves summarization outputs with-
  - of trustworthiness. ing to improve inherent model truthfulness.
  - To further reduce hallucinations and improve create a false sense of security, as subtle semantic

## 6. Conclusion、局限与可复现性

- **结论段落线索**：outcomes based on retrieved data). and categorize hallucinations. The Med-HALT benchmark (Pal et al., 2023) evaluates hallucina- Medical Natural Language Inference (Med- tion types using reasoning-based tests (e.g., 鈥淔alse NLI) Med-NLI analyzes the logical relationships Confidence鈥) and memory checks. In multimodal between medical texts. Key tasks include tex- settings, Med-HVL (Yan et al., 2024) distinguishes tual entailment (determining if one statement log- between Object Hallucination and Domain Knowl- ically follows another), contradiction detection edge Hallucination. (identifying conflicting statements), neutral rela- To mitigate hallucinations, post-hoc correction tionship identification (recognizing unrelated state- techniques are gaining traction. MEDAL (Li et al., ments), and causality recognition (inferring cause- 2024) presents a model-agnostic self-correction and-effect relationships). module that improves summarization outputs with- Medical Text Generation (Med-Gen) Med-
- **局限/未来工作线索**：fact-checking, multi-turn verification, and multi- hurt utility; future work needs fine-grained,；Limitations Evaluations remain fragmented: nar- Chen and Esmaeilzadeh (2024) offer a broader sur-；Limitations Current defenses (e.g., DP, redac- (GLiR), can detect whether individual patient data；Medical LLMs can still produce harmful or Limitations Mitigations often target generic
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“A Comprehensive Survey on the Trustworthiness of Large Language Models in Healthcare”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
