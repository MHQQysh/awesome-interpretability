# 063. Should You Mask 15% in Masked Language Modeling?

> 逐篇阅读记录：第 63 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Alexander Wettig, Tianyu Gao, Zexuan Zhong, Danqi Chen
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.eacl-main.217)
- **领域标签**：Probing, Representation, Hidden, LLM, Behavior, Analyze
- **本地 PDF 文本规模**：约 10,406 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Probing为主线，结合论文摘要中的核心设定：Masked language models (MLMs) conventionally mask 15% of tokens due to the belief that more masking would leave insufficient context to learn good representations; this masking rate has been widely used, regardless of mo
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：2 Background；4 MLMs in High-Masking Regimes of fine-tuning performance, which is substantially；2016. Neural machine translation of rare words with national Conference on Learning Representations
- **引言关键线索**：formance on downstream tasks, compared to the Pre-trained language models have transformed the default 15% masking (Table 1), and show consider- landscape of natural language processing (Devlin able performance in linguistic probing (搂4). This et al., 2019; Liu et al., 2019; Raffel et al., 2020; challenges common intuitions about masking rates Brown et al., 2020, inter alia). They are trained and what models learn in MLM pre-training. on vast quantities of text data and acquire rich and We then focus on the strategy of which tokens to versatile language representations. Compared to mask as an additional factor to the optimal masking autoregressive models, which always predict the rate of MLMs (搂5). We find that different mask- next token in a sequence, masked language models ing rates should be used with different masking * The first two authors contributed equally. 1 strategies, and the
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：considered at their optimal masking rate, uniform " # P masking achieves competitive performance. L(C) = E E log p(xi /x虄) , (1) x鈭圕 M鈯倄 Finally, we propose to dissect the masking rate /M/=m/x/ xi 鈭圡 into two factors (搂6): the corruption rate鈥攈ow where one masks m (masking rate, typically 15%) much of the context is corrupted (masked)鈥攁nd percentage of tokens from the original sentence x the prediction rate鈥攈ow much of the tokens the and predicts on the masked token set M given the model predicts on. In MLMs, both are set to the corrupted context x虄 (the masked version of x). masking rate. However, these two factors have Different masking strategies have been proposed opposing effects: higher prediction rates generate to sample M: Devlin et al. (2019) randomly more training signals and benefit the optimization, choose from the input tokens with a uniform dis- while higher corruption rates make the prediction tribution; Joshi et al. (202
- **方法与解释性关系**：该论文主要围绕 `Probing, Representation, Hidden, LLM, Behavior, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：On QNLI and QQP, In this section, However, Besides, Tay et al, Giampiccolo et al, Bentivogli C Tokenizer Comparison, Table 9, Comparison between BERT, Word-, Table 10, Comparison between our longer, RoBERTa, The tary material for, Figure 5
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - to achieve the same performance as the 15% baseline; On QNLI and QQP, the 40% model achieved the same
  - cient pre-training recipe and a comparison to other stant across different model sizes. In this section,
  - as the 15% baseline with only half the training 60
  - accuracy of the 15% model baseline. However, 15 20 30 40 15 20 30 40
  - ing as the baseline model (standard deviation reported), means the model learns from more training signals,
  - 2022b). Besides, Tay et al. (2022a) show that pre- 15% baseline on several tasks with half the training

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：未检测到稳定的百分比/倍数表达；请直接查看实验表格。
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - cally, we find that masking 40% outperforms
  - masking strategies such as span or PMI mask- (搂3). Specifically, we find that under an efficient
  - strategy. Together, our results contribute to a masking rates: if we mask as much as 80% of input
  - rate of MLMs (搂5). We find that different mask-
  - Together, our results demonstrate the overlooked
  - other hand, MLMs suffer a significant computa-
  - masked tokens with [MASK] by default. We find 3 Larger Models Can Benefit From

## 6. Conclusion、局限与可复现性

- **结论段落线索**：masking rate. gies adopt the optimal masking rates, the uniform We experiment with multiple masking strategies masking achieves similar and even better results as an additional factor for the optimal masking rate compared to the advanced strategies. We also re- in large models. Figure 5 shows the results of uni- mark that, when masking with 15%, simply increas- form masking, T5-style span masking (Raffel et al., ing the masking rate can be a more effective way 2020)6 , and PMI masking (Levine et al., 2021) un- to increase performance on SQuAD than switching der masking rates from 15% to 40%. We see that from uniform masking to another more advanced (1) for all masking strategies, the optimal masking strategy. More fine-grained results with these mask- 6 ing strategies are included in Appendix E. Span maskings in Raffel et al. (2020) and Joshi et al. (2020) differ in sampling procedures and we follow Raffel Interestingly, higher masking rates naturally in- et al. (2020) for implementati
- **局限/未来工作线索**：and prediction rates as future work. masked and removed from the input of the heavy；Limitations Scholar Award.；models and ELECTRA, and leave it for future work. Proceedings of the 60th Annual Meeting of the As-；for efficient pre-training to future work. learners. In Advances in Neural Information Pro-
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Should You Mask 15% in Masked Language Modeling?”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
