# 195. Bad Actor, Good Advisor: Exploring the Role of Large Language Models in Fake News Detection

> 逐篇阅读记录：第 195 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Beizhe Hu, Qiang Sheng, Juan Cao, Yuhui Shi, Yang Li, Danding Wang, Peng Qi
- **发表 venue / date**：AAAI / 2024/03
- **正式页面**：[Paper](https://ojs.aaai.org/index.php/AAAI/article/download/30214/32159)
- **领域标签**：Rationale, Evaluation, LLM, Behavior, Detect
- **本地 PDF 文本规模**：约 8,020 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决自然语言解释或归因结果是否忠实反映模型决策机制的问题。。方法上以Rationale为主线，结合论文摘要中的核心设定：Detecting fake news requires both a delicate sense of diverse clues and a profound understanding of the real-world background, which remains challenging for detectors based on small language models (SLMs) due to their kn
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Small；1) Though the LLM is generally believed powerful, the Statistical results by perspectives and cases are presented in；X Attention x Aggregator；2) The rationale-free ARG-D still outperforms all compared
- **引言关键线索**：et al. 2019) to understand news content and provide fun- The wide and fast spread of fake news online has posed real- damental representation, plus optional social contexts (Shu world threats in critical domains like politics (Fisher, Cox, et al. 2019; Cui et al. 2022), knowledge bases (Popat et al. and Hermann 2016), economy (CHEQ 2019), and public 2018; Hu et al. 2022b), or news environment (Sheng et al. health (Naeem and Bhatti 2020). Among the countermea- 2022) as supplements. SLMs do bring improvements, but sures to combat this issue, automatic fake news detection, their knowledge and capability limitations also compromise which aims at distinguishing inaccurate and intentionally further enhancement of fake news detectors. For example, misleading news items from others automatically, has been a BERT was pre-trained on text corpus like Wikipedia (De- promising solution in practice (S
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：periments on two real-world datasets demonstrate that ARG and ARG-D outperform three types of baseline methods, in- sense of diverse clues (e.g., style, facts, commonsense); and cluding SLM-based, LLM-based, and combinations of small 2) a profound understanding of the real-world background. and large language models. Recent methods (Zhang et al. 2021; Kaliyar, Goswami, and Narang 2021; Mosallanezhad et al. 2022; Hu et al. 2023) generally exploit pre-trained small language models Introduction (SLMs)1 like BERT (Devlin et al. 2019) and RoBERTa (Liu et al. 2019) to understand news content and provide fun- The wide and fast spread of fake news online has posed real- damental representation, plus optional social contexts (Shu world threats in critical domains like politics (Fisher, Cox, et al. 2019; Cui et al. 2022), knowledge bases (Popat et al. and Hermann 2016), economy (CHEQ 2019), and public 2018; Hu et al. 2022b), or news environment (
- **方法与解释性关系**：该论文主要围绕 `Rationale, Evaluation, LLM, Behavior, Detect` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：ARG-D outperform three types, LLM, SLM, Comparison between Small and, Large LMs, Baseline 0.753 0.754 0.769, Baseline + Rationale 0.767, Relative Impr. over Baseline, Evaluation Performance Comparison and, Ablation Study, Baselines We compare three, SLM-Only, Baseline, The vanilla BERT-base
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - and ARG-D outperform three types of baseline methods, in-
  - tribute the unsatisfying performance to the LLM鈥檚 inability approaches and perform a comparison with the SLM (here,
  - Comparison between Small and Large LMs
  - Baseline 0.753 0.754 0.769 0.737 0.765 0.862 0.916 0.615
  - Baseline + Rationale 0.767 0.769 0.787 0.748 0.777 0.870 0.921 0.633
  - (Relative Impr. over Baseline) (+4.2%) (+4.3%) (+4.6%) (+3.8%) (+3.2%) (+1.8%) (+1.1%) (+6.3%)

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：3.8%, 9.0%
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - and ARG-D outperform three types of baseline methods, in-
  - health (Naeem and Bhatti 2020). Among the countermea- 2022) as supplements. SLMs do bring improvements, but
  - and performing rule-based ensembles of judgments, we find tative LLM, i.e., GPT-3.5 in fake news detection to reveal
  - ARG and ARG-D outperform existing SLM/LLM-only and LLM developed by OpenAI and supporting the popular
  - 2) Few-shot versions outperform zero-shot ones, suggest- of the real-world background in fake news detection. 2) The
  - monsense perspective case). The results showcase that the
  - and the BERT. Results show that we are likely to gain a per- ft鈫抶 = AvgPool (CA(Rt , X, X)) , (2)

## 6. Conclusion、局限与可复现性

- **结论段落线索**：adaptive rationale guidance (ARG) network for fake news the understanding of the rationale texts. For the textual de- detection. Figure 3 overviews the ARG and its rationale-free scription rationale branch, we feed its representation Rt into version ARG-D, for cost-sensitive scenarios. The objective the LLM judgment predictor, which is parametrized using a of ARG is to empower small fake news detectors with the multi-layer perception (MLP)5 : ability to adaptively select useful rationales as references for m虃t = sigmoid(MLP(Rt )), Lpt = CE(m虃t , mt ), (4) final judgments. Given a news item x and its correspond- ing LLM-generated rationales rt (textual description) and rc where mt and m虃t are respectively the LLM鈥檚 claimed judg- (commonsense), the ARG encodes the inputs using the SLM ment and its prediction. The loss Lpt is a cross-entropy loss at first (Figure 3(a)). Subsequently, it builds news-rationale CE(y虃, y) = 鈭抷 log y虃鈭(1鈭抷) log(1鈭 y虃). The case is similar for commonsense ratio
- **局限/未来工作线索**：and capability limitations. Recent advances in large language (b) [News] - Commonsense: Real surgery；sures to combat this issue, automatic fake news detection, their knowledge and capability limitations also compromise；Limitations We identify the following limitations: 1) We
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Bad Actor, Good Advisor: Exploring the Role of Large Language Models in Fake News Detection”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
