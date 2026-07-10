# 072. Backpack Language Models

> 逐篇阅读记录：第 72 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：John K. Hewitt, John Thickstun, Christopher D. Manning, Percy Liang
- **发表 venue / date**：ACL / 2023/01
- **正式页面**：[Paper](https://aclanthology.org/2023.acl-long.506)
- **领域标签**：Representation, Evaluation, LLM, Behavior, Control
- **本地 PDF 文本规模**：约 13,848 个词

## 1. Abstract 讲解

- **研究问题**：研究试图建立模型内部表征、外部行为和可解释结论之间的稳定联系。
- **摘要主线**：解决大模型行为复杂、内部决策依据难以被人类理解的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：We present Backpacks: a new neural architecture that marries strong modeling performancewith an interface for interpretability and control.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：6 Sense Vectors for Control；8 Discussion of sense vectors to seemingly coherent categories；11 Limitations；12 Ethics Piotr Bojanowski, Edouard Grave, Armand Joulin, and；2021. Assessing the quality of human-generated sum-
- **引言关键线索**：cabulary as a set of non-contextual sense vectors Consider the prefix The CEO believes that ___, and that represent distinct learned aspects of the word. the problem of debiasing a neural language model鈥檚 For example, sense vectors for the word 鈥渟cience鈥 distribution over he/she. Intuitively, the bias for could encode types of science, connections to tech- he originates in the word CEO, because replacing nology, notions of science being 鈥渟ettled,鈥 or differ- CEO with nurse flips the observed bias. A success- ent aspects of the scientific process (replication or ful intervention to debias CEO must reliably apply experiment) (Table 1). Sense vectors do not learn in all contexts in which the word CEO appears; classic word sense, but more general aspects of a ideally we would want to make a non-contextual word鈥檚 potential roles in different contexts; in fact, change to the model that has pre
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：j=1 鈩=1 the-art specialized methods for this task. Finally, in Section 6 we show that sense vectors offer a control The contextualization weights 伪鈩搃j of a Backpack mechanism for Backpack language models. For ex- are themselves defined by a (non-linear) contextu- ample, stereotypically gendered profession words alization function of the entire sequence x1:n : (e.g., 鈥淐EO鈥 or 鈥渘urse鈥) tend to learn a sense vec- tor associated with this gender bias; by downscal- 伪 = A(x1:n ), (3) ing this sense vector, we greatly reduce disparity in contextual predictions in a limited setting. where A : V n 鈫 Rk脳n脳n . 9104 The name 鈥淏ackpack鈥 is inspired by the fact that center word: a backpack is like a bag鈥攂ut more orderly. Like a n X 1 bag-of-words, a Backpack representation is a sum v xc = vxi , (5) of non-contextual senses; but a Backpack is more i=1 n orderly, because the weights in this sum depend on p(xc / x1:n ) = softmax(U vxc ), (6) the ordered
- **方法与解释性关系**：该论文主要围绕 `Representation, Evaluation, LLM, Behavior, Control` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Equation, When oj is pa-, We train three Transformer, Embeddings from our models, Transformer, Except GPT-J-6B on RG-65, Sense 14, Baseline, To debias a Transformer, Results, We compare to Plug-and-Play, Language, Life and dies off, During the Street today
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - weight network in Equation (3). When oj is pa- We train three Transformer baseline models, which
  - Embeddings from our models + baseline Transformer (Except GPT-J-6B on RG-65). Sense 14, the 鈥渧erb
  - classifier assigns the correct topic label, and overall Baseline. To debias a Transformer with an analo-
  - Results. We compare to Plug-and-Play Language
  - Life and dies off in comparison to our own. During the Street today morning and observed a loading

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1.4x
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - sense vectors in this sequence. We find that,
  - ity evaluations, we find that Backpack sense their input. Almost any intervention on the model
  - vectors outperform even a 6B-parameter Trans- has complex, non-linear effects that depend on con-
  - ity with a larger model size. In Section 5, we show Weighted sum. For a sequence x1:n , the repre-
  - of a 170M parameter Backpack outperform word
  - Section 6 we show that sense vectors offer a control The contextualization weights 伪鈩搃j of a Backpack
  - for nouns. In Section 5.2 we show that sense 14

## 6. Conclusion、局限与可复现性

- **结论段落线索**：Comparing each Backpack LM to a Transformer sense 鈩 of a word x, and projecting this sense onto LM of equivalent specification to the Backpack鈥檚 the word embeddings: E 鈯 C(x)鈩 鈭 R/V/ . Note contextualization network, we see that the Back- that this is exactly (up to a scalar) how this sense pack performs roughly as well (Table 2). Again, the contributes to any prediction of the model. We in- Backpack has more parameters, a tax for the inter- terpret a sense vector鈥檚 role by reporting the words face provided by sense vectors. During training, we with the highest score under this projection. find that Backpack language models take longer to Table 3 visualizes a few of these senses. For converge than Transformers. Curiously, while the example, sense 12 seems to encode a broad no- Small Backpack and Transformer achieve almost tion of relatedness for almost all words; sense 3 identical OWT perplexity, the Backpack language encodes particulars of the bigram distribution given models perform 
- **局限/未来工作线索**：that future work will test larger model scales. In a vances in Neural Information Processing Systems,；3 A1. Did you describe the limitations of your work?
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Backpack Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
