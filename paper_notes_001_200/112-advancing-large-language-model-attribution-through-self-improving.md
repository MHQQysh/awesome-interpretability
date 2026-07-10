# 112. Advancing Large Language Model Attribution through Self-Improving

> 逐篇阅读记录：第 112 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Lei Huang, Xiaocheng Feng, Weitao Ma, Liang Zhao, Yuchun Fan, Weihong Zhong, Dan Xu, Qing Yang, et al.
- **发表 venue / date**：EMNLP / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.emnlp-main.223)
- **领域标签**：Attribution, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 10,101 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Lei Huang, Xiaocheng Feng, Weitao Ma, Liang Zhao, Yuchun Fan, Weihong Zhong, Dongliang Xu, Qing Yang, Hongtao Liu, Bing Qin.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction ing the inferior performance of LLMs in handling；2 Document 3；3 Problem Formulation and Methodology ing up, enabling the model to have the basic ability；5 Results；45 Table 4: Human evaluation results on attribution,；6 Human Evaluation；2023. Enabling large language models to generate；2023. Effective large language model adaptation for In our work, we use Llama-2-13b-base for data
- **引言关键线索**：the attribution task (Gao et al., 2023), generating The rapid development of large language models sufficient high-quality attribution responses solely (LLMs) (OpenAI, 2023; Zhao et al., 2023) has led through sampling proves difficult. This scarcity of to their prosperity as indispensable tools for infor- high-quality samples limits the opportunities for mation seeking. Despite their remarkable capabil- LLMs to self-improve effectively. Another chal- ity to generate fluent and informative responses to lenge stems from the limitation of weak supervi- user queries, LLMs also struggle with hallucina- sion signals. Current self-improvement approaches tions (Huang et al., 2023). To facilitate factuality (Yuan et al., 2023) primarily involve supervised verification, recent research (Bohnet et al., 2022) fine-tuning on high-quality samples while discard- has explored attributed text generation,
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：sponses1 (Li et al., 2024). However, acquiring atively improving the attribution capability of such data typically requires either manual cura- LLMs. First, to prevent models from stagnating tion (Malaviya et al., 2023), or distilled from the due to initially insufficient supervision signals, most advanced LLMs (Huang et al., 2024a,b), both S TART leverages the model to self-construct of which are costly and not scalable, thus limit- synthetic training data for warming up. To ing the growth of models鈥 attribution capability. further improve the model鈥檚 attribution abil- One promising solution is self-improvement (Yuan ity, S TART iteratively utilizes fine-grained pref- et al., 2023), which has demonstrated the poten- erence supervision signals constructed from its sampled responses to encourage robust, tial to boost model performance by learning from comprehensive, and attributable generation. self-generated high-quality samples. Experi
- **方法与解释性关系**：该论文主要围绕 `Attribution, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Table 1, Main result between our, Experiments are evaluated on, ASQA, ELI5, StrategyQA, For most baselines, Asai et al, Ye et al, Li et al, Baselines tion as a, We compare S TART, For a fair comparison, Main Results, Table 2, Ablation study results across, Table 3, The pass rate comparison
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - Table 1: Main result between our method and baselines. Experiments are evaluated on ASQA, ELI5, and StrategyQA
  - datasets. For most baselines, we use the result of previous works (Asai et al., 2023; Ye et al., 2023; Li et al., 2024).
  - 4.3 Baselines tion as a preference learning task, where they
  - We compare S TART with the following baselines. first supervised-fine-tuned on human-labeled high-
  - et al. (2023), given a query, we first instruct the For a fair comparison, all training-based baselines
  - quality data serves as a strong baseline to unlock 5.1 Main Results

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1X, 1 X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - Abstract sources, it can improve the explainability and cred-
  - in self-improvement that enhance LLMs with-
  - further improve the model鈥檚 attribution abil- One promising solution is self-improvement (Yuan
  - answering datasets, covering long-form QA tial of self-improvement in bootstrapping the at-
  - veals that S TART excels in aggregating infor- nation during the self-improvement process, pri-
  - LLMs to self-improve effectively. Another chal-
  - sion signals. Current self-improvement approaches

## 6. Conclusion、局限与可复现性

- **结论段落线索**：only the first iteration of self-improvement. As de- picted in Figure 3, training with synthetic data dur- We propose S TART, a self-improvement framework ing the initial iteration yields minimal performance to push the frontier of LLM attribution. We iden- gains. The attribution performance climbs slowly 5 We utilize gpt-3.5-turbo-0125 version. 3829 tify two key limitations for LLM attribution self- References improvement. To address these, S TART first lever- Akari Asai, Zeqiu Wu, Yizhong Wang, Avirup Sil, and ages self-constructed synthetic data for warming Hannaneh Hajishirzi. 2023. Self-rag: Learning to up, aiming to prevent models from early stagna- retrieve, generate, and critique through self-reflection. tion due to insufficient supervision signals. To ex- CoRR, abs/2310.11511. plore more fine-grained supervision signals, S TART Bernd Bohnet, Vinh Q. Tran, Pat Verga, Roee Aharoni, constructs fine-grained preference supervision sig- Daniel Andor, Livio Baldini Soares, Jacob Eise
- **局限/未来工作线索**：lenge stems from the limitation of weak supervi-；high-quality samples. This limitation poses sub-；exhibits remarkable effectiveness in improving its that this limitation stems from the inherent diffi-；tify two key limitations for LLM attribution self- References
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Advancing Large Language Model Attribution through Self-Improving”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
