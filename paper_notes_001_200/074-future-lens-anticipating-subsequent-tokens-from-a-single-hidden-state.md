# 074. Future Lens: Anticipating Subsequent Tokens from a Single Hidden State

> 逐篇阅读记录：第 74 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Koyena Pal, Jiuding Sun, Andrew C. Yuan, Byron Wallace, David Bau
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.conll-1.37)
- **领域标签**：Representation, Lens, MLLM, Hidden, Layer, Evaluate
- **本地 PDF 文本规模**：约 7,534 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决多模态大模型视觉-语言证据如何被使用和对齐难以解释的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：We conjecture that hidden state vectors corresponding to individual input tokens encode information sufficient to accurately predict several tokens ahead.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction the future, in order to reveal the extent to which；2 Methods；5 Discussion better by the learned prompt than by the linear；2023. Preventing verbatim memorization in lan-；2023. Evaluating the zero-shot robustness of；2021. BERxiT: Early exiting for BERT with better
- **引言关键线索**：Do hidden states in large language models (LLMs) individual hidden states may directly encode sub- encode tokens farther than a single token ahead? sequent tokens. Second, we perform a causal in- If so, how can we decode this sequence of tokens tervention study in which we transplant individual from a single state? In this work we empirically hidden states from one context to a completely investigate these questions using GPT-J-6B (Wang different context and measure the extent to which and Komatsuzaki, 2021). We train models to pre- future tokens that were predicted in the original dict hidden states several tokens ahead of a given context can be predicted in the foreign context. Fi- position t based only on a contextualized represen- nally, we fit a 鈥渟oft prompt鈥 to explicitly learn an tation of the input at this position. optimal prompt that permits reading out informa- Auto-regressive
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：the degree to which individual hidden states output of the model when compared against their in the network contain signal rich enough to fully computed model run. predict future hidden states and, ultimately, to- In this work we ask: To what extent can we ex- ken outputs. We find that, at some layers, we tract information about future (beyond subsequent) can approximate a model鈥檚 output with more tokens from a single hidden token representation? than 48% accuracy with respect to its predic- To answer this, we conduct three experiments. First, tion of subsequent tokens through a single hid- extending the ideas of Tuned Lens (Belrose et al., den state. Finally we present a 鈥淔uture Lens鈥 visualization that uses these methods to create 2023; Din et al., 2023) and the Logit lens (nostal- a new view of transformer states. gebraist, 2020), we train linear models to approx- imate future model predictions several tokens in
- **方法与解释性关系**：该论文主要围绕 `Representation, Lens, MLLM, Hidden, Layer, Evaluate` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：The learned tions contribute, For next-token predictions. Katz, Belinkov, Figure 4, The bigram baseline is, While we did create
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - predictive accuracy peaking at middle layers, with more than double the accuracy of a bigram baseline. A linear
  - tween our method and the baselines. The learned tions contribute the most to the final predicted to-
  - 25.1% higher than the best baseline method. For next-token predictions. Katz and Belinkov (2023)
  - baseline at N = 1 (the horizontal line in Figure 4).1
  - The bigram baseline is collected from 900,000 documents the best probing method performed specifically for
  - baselines we would refer to. While we did create a bigram baseline in the case of predicting 2 tokens in

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - ken outputs. We find that, at some layers, we tract information about future (beyond subsequent)
  - To predict yT +N from hlT alone, we train a linear of the transformer. In Figure 1, we show an ex-
  - the transformer. We find that this marginally aids yT = G(x) 鈭 [0, 1]/V / (7)
  - we show the original context from which hlT is The context prompt c could be chosen as any
  - produces a significantly better optimized context. 3.3 Experimental Setup
  - model, the relative improvement is highest when
  - future tokens. In Figure 6, we show an example insights about the model鈥檚 chain of predictions at

## 6. Conclusion、局限与可复现性

- **结论段落线索**：model, the relative improvement is highest when In this paper we explored the degree to which we the last context token is a lowercase token preceded are able to decode multi-token outputs subsequent by a space, or a longer token. This suggests that to a particular token on the basis of its hidden information about how to complete long words may representation alone. The results in Table 2 and not be immediately accessible by a linear model Figures 4 and 5 indicate that such representations decoder, but that they can be made accessible by encode such information, at least to some degree. using the parameters of the pretrained model as Among the decoding methods we assessed, learned done by the learned prompt intervention method. prompts are best able to predict such future tokens. We have also observed that the accuracy of pre- Both the linear and the learned prompt models dicting subsequent tokens is correlates with the achieve better accuracy than the empirical bigram model鈥檚 confide
- **局限/未来工作线索**：未稳定识别 limitations 小节；需要结合作者讨论和附录核对。
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Future Lens: Anticipating Subsequent Tokens from a Single Hidden State”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
