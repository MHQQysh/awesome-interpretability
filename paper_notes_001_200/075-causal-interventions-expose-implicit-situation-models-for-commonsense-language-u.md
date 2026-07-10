# 075. Causal interventions expose implicit situation models for commonsense language understanding

> 逐篇阅读记录：第 75 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Takateru Yamakoshi, James L. McClelland, Adele Ε. Goldberg, Robert D. Hawkins
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.findings-acl.839)
- **领域标签**：Mechanistic, Transformer, Behavior, Control
- **本地 PDF 文本规模**：约 14,053 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型具体计算路径、关键模块和因果机制难以拆解的问题。。方法上以Mechanistic为主线，结合论文摘要中的核心设定：Accounts of human language processing have long appealed to implicit “situation models” that enrich comprehension with relevant but unstated world knowledge.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；2 MASK mask MASK；21 Second, situation models present a well-known；1994. Constructing inferences during narrative text on Natural Language Processing (Volume 1: Long
- **引言关键线索**：Modern large language models (LLMs) exhibit Language understanding is deeply intertwined with increasingly impressive performance on 鈥渃ommon- world knowledge. For example, when reading a sense鈥 tasks that seemingly require the use of im- sentence like 鈥渢he fish ate the worm,鈥 we can guess plicit world knowledge (Sap et al., 2019; Zellers that the fish was probably hungrier before eating et al., 2019; Petroni et al., 2019; Davison et al., and that the worm is no longer alive, even though 2019; Vulic虂 et al., 2020), yet it is still not clear neither property is explicitly mentioned (Winograd, precisely how that knowledge is accessed and em- 1972; Rumelhart, 1975). Classical psycholinguis- ployed. Recent interpretability work has explicitly tic accounts have suggested that such knowledge probed and traced individual pieces of world knowl- enters into language understanding through struc- ed
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：transformer model (ALBERT; Lan et al., 2020) plicit situation model representations with a more on a Winograd-like pronoun disambiguation task targeted set of causal intervention techniques, trac- (Winograd, 1972; Levesque et al., 2011; Sakaguchi ing the internal flow of subtle contextual cues in et al., 2021, see Figure 1). Winograd sentences Winograd schemas for the first time. are minimal pairs constructed with the property 2.2 Probing with causal interventions that resolving an ambiguous pronoun requires situ- ational world knowledge outside the scope of the In order to identify interpretable algorithms under- literal text; critically, for our purposes, the pair of lying specific model behaviors, recent studies have sentences differs only at one site, which causes employed variants of causal intervention analyses the interpretation to flip. Controlling for possible on intermediate representations, targeting syntactic confounds, a mo
- **方法与解释性关系**：该论文主要围绕 `Mechanistic, Transformer, Behavior, Control` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：In a, Finally, To facilitate comparison across, Kocijan et al, Trichelair et al, API, So far, To build intuition, In this paper
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - additional conditions for comparison. In a 鈥渃on- each noun phrase (NA , NB ) at the masked posi-
  - Finally, we generated a control condition using late the baseline preference for the correct referent
  - To facilitate comparison across sentences of different
  - baseline prediction is correct, but depending on 2020; Kocijan et al., 2022; Trichelair et al., 2019).
  - probability of the correct referent, the causal effect we conduct our own stricter zero-shot comparison
  - models perform better overall for the syntax cue the baseline predictions are 鈥渨eakly鈥 correct (see

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - identified situation model circuit. Overall, we find attention heads appears to identify indirect objects,
  - is 82%, compared to 87% human accuracy). only yielded a significantly non-zero effect on the
  - number of sentences for which they make the cor- dicted, no significant effects were observed when
  - no significant effects, indicating that heads are not the target site (see Figure S4 for a schematic).
  - how much pronoun-relevant context-sensitivity is Moving to a more systematic analysis, we find
  - that layer overall, which can then be broken down total heads) show a significant effect of interven-
  - analysis above, we find that intervening on trans- gate to the masked site, aggregating over many

## 6. Conclusion、局限与可复现性

- **结论段落线索**：tive heads in Figure 6, where specificity is defined with respect to the context vs. syntax comparison: In this paper, we presented a preliminary investi- an orange cell indicates 鈥榗ontext-specificity鈥 (i.e. gation of the transformer circuits underlying per- selective activation only in the context condition) formance on Winograd sentences, where minimal while a blue cell indicates 鈥榮yntax-specificity鈥 (i.e. contextual cues must be used to resolve an ambigu- 13272 rest masks options anything like a situation model (but see Appendix 42 D for an additional analysis suggesting that this 11 23 kind of phenomenon is unlikely to be driving the 3 8 observed effects).
- **局限/未来工作线索**：dition, and black cells for both conditions. All colored ior, this technique has known limitations addressed；3 A1. Did you describe the limitations of your work?
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Causal interventions expose implicit situation models for commonsense language understanding”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
