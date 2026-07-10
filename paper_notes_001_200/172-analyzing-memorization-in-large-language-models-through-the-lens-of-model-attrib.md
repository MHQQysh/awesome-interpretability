# 172. Analyzing Memorization in Large Language Models through the Lens of Model Attribution

> 逐篇阅读记录：第 172 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Tarun Ram Menta, Susmit Agrawal, Chirag Agarwal
- **发表 venue / date**：NAACL / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.naacl-long.535)
- **领域标签**：Attribution, Lens, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 13,879 个词

## 1. Abstract 讲解

- **研究问题**：模型输出可能不事实、不忠实或无法可靠归因，研究需要把这些风险转化为可评测、可解释的对象。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Attribution为主线，结合论文摘要中的核心设定：Tarun Ram Menta, Susmit Agrawal, Chirag Agarwal.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；3 Methodology；2 Related Work；2) Token Accuracy: Samples may not be verbatim；4 Experiments；300 Original Model；5 Conclusion；3. Study on targeted extraction: While our work
- **引言关键线索**：of specific attention modules on both memorization Large Language Models (LLMs) have become and generalization. We support our experimental ubiquitous in modern applications, powering every- findings with theoretical analysis of the impact of thing from conversational agents to advanced data bypassing attention modules at different blocks. * Equal Contribution. Work done by the first two authors as Our theorems introduce bounds on the difference Visiting Researchers in Aikyam Lab, University of Virginia. in model outputs with standard attention and short- 1 https://github.com/AikyamLab/llm-memorization circuited attention. Our theoretical insights and 10661 Proceedings of the 2025 Conference of the Nations of the Americas Chapter of the Association for Computational Linguistics: Human Language Technologies (Volume 1: Long Papers), pages 10661鈥10689 April 29 - May 4, 2025 漏2025 Associatio
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：memorization metrics鈥攚ithout exploring the hoc perspective, where they predominantly focus underlying architectural factors contributing on methods to extract memorized samples from to memorization. In this work, we investigate trained LLMs (Carlini et al., 2020; Huang et al., memorization from an architectural lens 2022) or propose metrics to quantify memoriza- by analyzing how attention modules at different layers impact its memorization and tion (Carlini et al., 2022; Ishihara and Takahashi, generalization performance. Using attribution 2024). While some works also explore machine un- techniques, we systematically intervene in learning techniques to remove specific memorized the LLM鈥檚 architecture by bypassing attention content after training (Huang et al., 2024), they do modules at specific blocks while keeping other not delve into the fundamental model mechanisms components like layer normalization and MLP that contribute to memori
- **方法与解释性关系**：该论文主要围绕 `Attribution, Lens, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：正文中存在 baseline/comparison 讨论，但文本提取未稳定识别名称。
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：未自动提取到稳定短句，需回看论文中的 Baselines、Comparison 或 Ablation 小节。

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1X, 10672
                                                                          X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - deeper blocks results in smaller differences in the patterns are observed. In our work, we show
  - content verbatim and do not significantly impair its pertinent for memorization, while not boosting the
  - tion allows for precise analysis of different without significant performance loss. To our knowl-
  - layers 18-24 for GPTNeo-1.3B) leads to a sig- tion and leads to a significant drop of benchmark
  - short-circuited variants even outperform the bench- applying attention short-circuiting in the first block
  - significant drop in memorization, and short-circuiting the last quartile still maintains a large portion of performance
  - 0.05 memorization and has an insignificant effect on the

## 6. Conclusion、局限与可复现性

- **结论段落线索**：未稳定识别 Conclusion 段落。
- **局限/未来工作线索**：2023; Yu et al., 2023) is left as future work.；While exact memorization, as defined in the pre- targeted extraction techniques as future work.；0.0 6 Limitations and Future Directions；Figure 7: Relative decrease in benchmark performance eral limitations warrant further investigation:
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Analyzing Memorization in Large Language Models through the Lens of Model Attribution”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
