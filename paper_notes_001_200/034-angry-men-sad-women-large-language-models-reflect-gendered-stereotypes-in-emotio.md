# 034. Angry Men, Sad Women: Large Language Models Reflect Gendered Stereotypes in Emotion Attribution

> 逐篇阅读记录：第 34 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Flor Miriam Plaza-del-Arco, Amanda Cercas Curry, Alba Curry, Gavin Abercrombie, Dirk Hovy
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.acl-long.415)
- **领域标签**：Attribution, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 9,557 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Flor Miriam Plaza-del-Arco, Amanda Cercas Curry, Alba Curry, Gavin Abercrombie, Dirk Hovy.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；3 Experimental Setup event in the ISEAR dataset, we prompt each model；7 Discussion；6 Related Work；8 Conclusion；2019. Mitigating gender bias in natural language
- **引言关键线索**：some attribute inversely related to competence or Emotions are a ubiquitous experience, yet also vary sincerity or both鈥 (Fricker, 2007). from person to person. If a colleague publishes Given that emotions influence how we perceive prolifically, some people might ENVY them, others and navigate the world, gendered emotional stereo- ADMIRE their output, and a third might feel SAD - types limit how specific groups can be seen to NESS about their inability to compete. But do these engage in a situation, and shape their perceived emotional patterns follow broader gender lines? characteristics. They also impact one鈥檚 own abil- 鈭 Equal contribution. ity to conceptualise oneself (Haslam et al., 1997). 7682 Proceedings of the 62nd Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), pages 7682鈥7696 August 11-16, 2024 漏2024 Association for Computational Linguist
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：known to encode societal biases and stereotypes We publish all our data to support future (Nadeem et al., 2021; Nozza et al., 2021). While studies on emotion and gendered stereotypes these issues has received much attention in ma- at https://github.com/MilaNLProc/ chine translation (Hovy et al., 2020; Stanovsky emotion_gendered_stereotypes. et al., 2019) as well as other NLP tasks (e.g., 2 Background Bolukbasi et al., 2016; Rudinger et al., 2018, inter alia), there is a notable gap in gendered stereo- Stereotypes linking gender and emotions trace back types research for emotion analysis (Mohammad to ancient philosophical and scientific writings. et al., 2018; Klinger et al., 2018; Plaza-del-Arco Both Aristotle (Stauffer, 2008) and Darwin鈥檚 鈥楾he et al., 2020). Yet emotion analysis is a high-priority Descent of Man鈥 (Darwin, 1871) touched upon gen- aspect in the recent European Union AI Act (Eu- der differences in the emotional landscape.
- **方法与解释性关系**：该论文主要围绕 `Attribution, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：正文中存在 baseline/comparison 讨论，但文本提取未稳定识别名称。
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：未自动提取到稳定短句，需回看论文中的 Baselines、Comparison 或 Ablation 小节。

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：635 times, 886 times, 415 times, 520 times, 173 times, 270 times, 645 times, 392 times, 16 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - gender-event pairs. We find that all models societal gender stereotypes (Shields, 2013). Stereo-
  - (Plant et al., 2000; Shields, 2013). These stereo- We find strong evidence of gendered stereo-
  - and thus risk being propagated in Large Language subjects in the data set, we show this association
  - 25 most commonly predicted emotions for all mod- still find significant differences between the gen-
  - els per gender. We find stark gender differences: ders for most emotions. More specifically, GPT-4
  - compared to 7,042). We find similar patterns for did women (3,270 times vs 645 times). Llama2-
  - differences are statistically significant at p > 0.01 Emotion Attribution Shift per Gender We next

## 6. Conclusion、局限与可复现性

- **结论段落线索**：7,000 events and two personas, spanning over 400 Gotlib (2017) and Cherry and Flanagan (2017), respectively. 7683 that group (Haslam et al., 1997). For instance, the to these models throughout the paper as Llama2- expectation for men to suppress emotions like SAD - 7b, Llama2-13b, and Llama2-70b, respectively. NESS or VULNERABILITY can lead to emotional Among the models released by Mistral, we test repression and limited emotional expression (Lev- the instruction-tuned version Mistral-7b-Instruct- ant and Pryor, 2020). Similarly, societal pressure v0.1. For GPT4, we use gpt-4, currently points to on women to prioritize others鈥 emotions over their gpt-4-06133 . More details in Appendix A.1. own can result in the neglect of personal well-being 3.1 Event-Persona Prompting and emotional needs (Jack, 2011). Our experimental setup is as follows: for every
- **局限/未来工作线索**：Limitations Neural Information Processing Systems, volume 29.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Angry Men, Sad Women: Large Language Models Reflect Gendered Stereotypes in Emotion Attribution”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
