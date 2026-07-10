# 177. On Large Language Models’ Hallucination with Regard to Known Facts

> 逐篇阅读记录：第 177 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Che Jiang, Biqing Qi, Xiangyu Hong, Dayuan Fu, Yang Cheng, Fandong Meng, Mo Yu, Bowen Zhou, et al.
- **发表 venue / date**：NAACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.naacl-long.60)
- **领域标签**：Analysis, Hallucination, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 7,446 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决大模型幻觉或事实性错误难以定位、解释和诊断的问题。。方法上以Analysis为主线，结合论文摘要中的核心设定：Che Jiang, Biqing Qi, Xiangyu Hong, Dayuan Fu, Yang Cheng, Fandong Meng, Mo Yu, Bowen Zhou, Jie Zhou.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2. MLP modules have a more significant impact behavior in models, revealing a gap between causal；4. The dynamic patterns of output tokens can be knowledge, numerous studies have analyzed the；5 Conclusion；6 Limitations han, Mohammad Aflah Khan, Shivanshu Purohit,
- **引言关键线索**：propose self-assessment methods for models to Large Language Models (LLMs) have shown great rectify this form of hallucination (Kadavath et al., potential to acquire extensive knowledge and apply 2022; Yin et al., 2023; Shuster et al., 2021). The it in various tasks (Petroni et al., 2019; AlKhamissi second scenario occurs when the model鈥檚 param- et al., 2022; Cohen et al., 2023). Despite their eters memorize relevant knowledge but lack gen- proficiency in generating coherent and contextually eralization ability. Prompt engineering or prefix relevant text, these models frequently manifest 鈥檉ac- tuning can partially alleviate these factual halluci- tual hallucinations,鈥 significantly undermining their nations, enhancing the model鈥檚 performance on spe- reliability in practical applications (Zhang et al., cific tasks (Zhong et al., 2021; Youssef et al., 2023). 鈭 However, the mechanism behind
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：Large Language Models (LLMs) have shown great rectify this form of hallucination (Kadavath et al., potential to acquire extensive knowledge and apply 2022; Yin et al., 2023; Shuster et al., 2021). The it in various tasks (Petroni et al., 2019; AlKhamissi second scenario occurs when the model鈥檚 param- et al., 2022; Cohen et al., 2023). Despite their eters memorize relevant knowledge but lack gen- proficiency in generating coherent and contextually eralization ability. Prompt engineering or prefix relevant text, these models frequently manifest 鈥檉ac- tuning can partially alleviate these factual halluci- tual hallucinations,鈥 significantly undermining their nations, enhancing the model鈥檚 performance on spe- reliability in practical applications (Zhang et al., cific tasks (Zhong et al., 2021; Youssef et al., 2023). 鈭 However, the mechanism behind the model鈥檚 hal- The work was done when Che Jiang worked as intern at Pattern Recognition Cente
- **方法与解释性关系**：该论文主要围绕 `Analysis, Hallucination, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Geva et al, Consequently, The comparison between the, Logit Lens mapping. Subsequently, This plays a crucial, In comparison to successful, We compared three curves, SVM clas-, Firstly
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - quently, we compare the inference dynamics under head (Geva et al., 2023). Consequently, errors oc-
  - The comparison between the probability shifts under the Logit Lens mapping. Subsequently, we
  - contributes to errors in knowledge recall. This plays a crucial role. In comparison to successful
  - We compared three curves as vectors for SVM clas-
  - edgment. Firstly, to guarantee fair comparison for

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - tual hallucinations,鈥 significantly undermining their nations, enhancing the model鈥檚 performance on spe-
  - not know (Yin et al., 2023; Turpin et al., 2023), recalling or hallucinating. We show the dynamic
  - inference, which is significantly lower than the ference (Meng et al., 2022a,b). But the addition
  - 2. MLP modules have a more significant impact behavior in models, revealing a gap between causal
  - We observe three types of token variation curves Does a subject鈥檚 popularity significantly influ-
  - knowledge. significant correlation between these error types
  - equally significant contributions to knowledge ex-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Acknowledgements We have analyzed the scenario where LLMs hal- lucinate on known facts. Using our dataset based This work is supported by the National Science and on triplet knowledge, we make comparative obser- Technology Major Project (No. 2022ZD0117903). vations of the model鈥檚 reasoning dynamics across We extend our gratitude to the anonymous review- various outputs. We show that the cause of hallu- ers for their insightful feedback. We thank Kaiyan cinations lies in the failure of factual recall. The Zhang, Ermo Hua, Zixu Hao and Yiyao Jiang for failure may result from a bias in subject parsing, their helpful comments. leading to inadequate extraction of object-related information at higher levels. This information then competes with hallucinatory information flow and References gets suppressed by MLPs. Based on the distinct dif- Guillaume Alain and Yoshua Bengio. 2016. Under- ferences observed in the dynamics of reasoning, we standing intermediate layers using linear classifier tr
- **局限/未来工作线索**：1 Introduction bases when faced with such limitations. Studies；rameter in future work. For all the probability varia- put token dynamics, predicting whether the four；aging the findings of this study, future work could Diab, and Marjan Ghazvininejad. 2022. A review on；6 Limitations han, Mohammad Aflah Khan, Shivanshu Purohit,
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“On Large Language Models’ Hallucination with Regard to Known Facts”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
