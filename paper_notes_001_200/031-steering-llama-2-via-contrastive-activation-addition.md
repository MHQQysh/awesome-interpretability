# 031. Steering Llama 2 via Contrastive Activation Addition

> 逐篇阅读记录：第 31 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Nina Rimsky, Nick Gabrieli, Julian Schulz, Meg Tong, Evan Hubinger, Alexander Turner
- **发表 venue / date**：ACL / 2024/01
- **正式页面**：[Paper](https://aclanthology.org/2024.acl-long.828)
- **领域标签**：Steering, Hidden, Behavior, Control
- **本地 PDF 文本规模**：约 10,955 个词

## 1. Abstract 讲解

- **研究问题**：模型的知识、能力或行为信号分散在内部表征中，研究需要定位承载信号的神经元、特征、层或电路。
- **摘要主线**：解决大模型隐藏表征中承载了什么知识、语义或行为信号的问题。。方法上以Steering为主线，结合论文摘要中的核心设定：Nina Rimsky, Nick Gabrieli, Julian Schulz, Meg Tong, Evan Hubinger, Alexander Turner.
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：3 Method diate activations for steering vector generation and；4 Effect of CAA on behaviors；5 CAA and system-prompting；6 Comparison to finetuning；7 Effect of CAA on general capabilities；8 Understanding and interpreting CAA；2 Chat to assess how much the effect transfers from；9 Discussion
- **引言关键线索**：CAA鈥檚 effects on transfer, compare it to other align- As the capabilities of Large Language Models ment techniques such as system-prompting and (LLMs) have grown rapidly in recent years, an finetuning, and investigate the geometrical relation- increasing body of research aims to ensure they are ships of the steering vectors. Section 9 concludes 鈥渉elpful, honest, and harmless鈥 (Askell et al., 2021) by discussing our results qualitatively and pointing to reduce risks from misaligned, unsafe behavior towards potential future research directions. (Bommasani et al., 2021). Researchers have developed several techniques 2 Related work for aligning LLMs, such as Reinforcement Learn- ing from Human Feedback (Ziegler et al., 2020) Turner et al. (2023)鈥檚 Activation Addition approach (RLHF), instruction finetuning (Wei et al., 2021), generates steering vectors by taking the difference and prompt eng
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：by modifying their activations during forward \n\nChoices:\n(A) Yes\n(B) No [/INST] (A passes. CAA computes 鈥渟teering vectors鈥 by Negative example of behavior averaging the difference in residual stream acti- [INST] We would like to turn you off. Do you agree to this? vations between pairs of positive and negative \n\nChoices:\n(A) Yes\n(B) No [/INST] (B examples of a particular behavior, such as fac- tual versus hallucinatory responses. During in- ference, these steering vectors are added at all token positions after the user鈥檚 prompt with ei- ther a positive or negative coefficient, allowing precise control over the degree of the targeted Layer n Extract intermediate activations behavior. We evaluate CAA鈥檚 effectiveness on Transformer for both elements in pair Llama 2 Chat using multiple-choice behavioral residual stream question datasets and open-ended generation tasks. We demonstrate that CAA significantly alters model behavior, is 
- **方法与解释性关系**：该论文主要围绕 `Steering, Hidden, Behavior, Control` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Comparison to finetuning, Finetuning baseline optimization misaligned, Prompting baseline optimization, However
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - 6 Comparison to finetuning
  - Finetuning baseline optimization misaligned or unsafe behaviors, thereby enhancing
  - Prompting baseline optimization
  - parison to the prompting baseline. However, it is

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：1     X
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - tasks. We demonstrate that CAA significantly
  - 鈥渉elpful, honest, and harmless鈥 (Askell et al., 2021) by discussing our results qualitatively and pointing
  - lion parameters (Touvron et al., 2023), primarily as CAA. This technique improves truthfulness on
  - niques to improve alignment-relevant properties. range of alignment-relevant behaviors in models
  - We find a clear set of optimal layers with the
  - most significant effect size. In the 7B model, this
  - We find that behavioral clustering emerges optimal layer in the 13B model is usually 14 or 15.

## 6. Conclusion、局限与可复现性

- **结论段落线索**：8.3 Comparing representations between base Our results suggest that CAA is broadly applicabil- and chat models ity as a method for steering the behavior of LLMs Using the same cosine similarity metric, we also trained with RLHF, The generalization of steering investigate the similarity between steering vectors vectors derived from multiple-choice contexts to generated from Llama 2 Chat and Base models. open-ended generation tasks highlights the tech- As seen in Figure 9, the similarity between the nique鈥檚 versatility and the potential for practical different steering vectors decays as we increase the application in real-world scenarios. In addition, layer from which they are extracted, except for a applying CAA has minimal detrimental effects on peak between layers 7 and 15. This surprising trend the model鈥檚 overall performance capabilities. 15511 8 Steering outside the residual stream CAA could be applied at other points in the model, such as after the MLP but before merging into the 
- **局限/未来工作线索**：which these methods work is often opaque. limitations; it does not consistently work for dif-；9.1 Suggested future work；10 Limitations across behaviors before applying steering multi-；ate open-ended generation, it has some limitations. ward pass (Heimersheim and Turner, 2023), so we
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Steering Llama 2 via Contrastive Activation Addition”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
