# 090. Back Attention: Understanding and Enhancing Multi-Hop Reasoning in Large Language Models

> 逐篇阅读记录：第 90 篇 / 200。以下内容基于论文 PDF 文本、正式元数据和该论文的摘要；方法、baseline 和 finding 的具体数值应以原文表格为最终依据。

## 0. 论文信息

- **作者**：Zeping Yu, Yonatan Belinkov, Sophia Ananiadou
- **发表 venue / date**：EMNLP / 2025/01
- **正式页面**：[Paper](https://aclanthology.org/2025.emnlp-main.567)
- **领域标签**：Representation, Evaluation, Reasoning, Hidden, Layer, Analyze
- **本地 PDF 文本规模**：约 9,660 个词

## 1. Abstract 讲解

- **研究问题**：模型推理过程难以观察和验证，研究需要比较推理链、结构化证据和最终答案之间是否一致。
- **摘要主线**：解决大模型推理过程不透明、正确性信号难以从内部状态读出的问题。。方法上以Representation为主线，结合论文摘要中的核心设定：We investigate how large language models (LLMs) perform latent multi-hop reasoning in prompts like "Wolfgang Amadeus Mozart's mother's spouse is".To analyze this process, we introduce logit flow, an interpretability meth
- **阅读解释**：摘要通常完成“现象/缺口 -> 方法 -> 实验对象 -> 结论”的压缩叙述。阅读这篇论文时，应把摘要中的 claim 拆成可验证的实验问题，而不要把摘要里的提升直接当成跨模型结论。

## 2. Introduction 讲解

- **引言结构**：1 Introduction；4 Mechanism of Two-Hop Prediction；5 Back Attention: Letting Lower Layers；9 Acknowledgements；7 Conclusion This work was supported by the computational；2023 Conference on Empirical Methods in Natural izing and interpreting the semantic information flow；2025. Lost in multilinguality: Dissecting cross- Least-to-most prompting enables complex reason-
- **引言关键线索**：e1 position and the last position inherently obtain Enhancing the multi-hop reasoning capabilities of the information of r1 and r2, making it unsurpris- large language models (LLMs) has become a cen- ing that information flows between them. A more tral research focus in recent studies (OpenAI, 2024; complex format, 鈥渆1鈥檚 r1鈥檚 r2 is鈥, introduces addi- Qi et al., 2024; Snell et al., 2024; Luo et al., 2024). tional challenges. Due to the autoregressive nature A widely used approach, chain-of-thought (COT) of decoder-only LLMs, earlier positions cannot ac- reasoning (Wei et al., 2022), improves accuracy cess later tokens, hindering relational knowledge by explicitly articulating intermediate reasoning propagation and leading to lower accuracy than steps. Many studies have expanded on this idea 鈥淭he r2 of the r1 of e1 is鈥 prompts. Second, sev- by generating explicit reasoning chains to furthe
- **缺口与贡献的读法**：重点区分作者提出的新测量、新模型、新数据集、新干预，还是把已有解释工具应用到新任务；这决定论文属于方法创新、评测创新还是应用研究。

## 3. Method / Framework 讲解

- **方法段落线索**：model鈥檚 ability to internally retrieve and integrate layers and positions toward the final predic- tion. Using logit flow, we identify four distinct relevant knowledge. Recent studies have investi- stages in single-hop knowledge prediction: (A) gated the mechanisms underlying latent multi-hop entity subject enrichment, (B) entity attribute reasoning. Given two hops <e1, r1, e2> and <e2, r2, extraction, (C) relation subject enrichment, and e3>, where 鈥渆鈥 represents an 鈥渆ntity鈥 and 鈥渞鈥 a 鈥渞e- (D) relation attribute extraction. Extending this lation鈥, Yang et al. (2024) observe that LLMs can analysis to multi-hop reasoning, we find that sometimes successfully predict queries like 鈥淭he failures often stem from the relation attribute r2 of the r1 of e1 is鈥 -> 鈥渆3鈥 by latently identify- extraction stage, where conflicting logits re- ing the bridge entity 鈥渆2鈥. However, Biran et al. duce prediction accuracy. To address this, we propose back at
- **方法与解释性关系**：该论文主要围绕 `Representation, Evaluation, Reasoning, Hidden, Layer, Analyze` 展开；应追踪输入、内部状态/解释单元、干预或评分函数、最终输出之间的数据流。
- **关键检查点**：解释单元是 token、layer、attention head、MLP、neuron、SAE feature、rationale、source document 还是外部知识；不同单元不能直接横向比较。

## 4. Baseline 与对比讲解

- **检测到的 baseline / comparison 关键词**：Instead, We compare the, Table 1. Compared with, Figure 11. Compared with, We use the AdamW, Loshchilov, The comparison of correct
- **对比维度**：通常需要同时看任务性能、解释质量/faithfulness、计算成本、扰动后的稳定性和副作用；只看主任务分数会掩盖解释方法的代价。
- **正文对比证据索引**：
  - 鈥淟eopold鈥. Instead, at the entity position, lower e2, e3 are all human entities. We compare the
  - e1鈥檚 r1鈥檚 r2 is steady decline compared with higher layers. We
  - fine-tuning on the double-sum arithmetic cases. reported in Table 1. Compared with zero-shot ac-
  - are shown in Figure 11. Compared with the correct We use the AdamW optimizer (Loshchilov, 2017)
  - The comparison of correct and false human-

## 5. Experiments 与 Findings 讲解

- **可检测的数值信号**：30 times
- **结果解读顺序**：先确认数据集、模型、prompt、评价器和预算是否与 baseline 完全一致，再判断提升来自方法本身还是协议差异。
- **正文 finding 证据索引**：
  - analysis to multi-hop reasoning, we find that sometimes successfully predict queries like 鈥淭he
  - back attention improves accuracy on five rea-
  - reasoning (Wei et al., 2022), improves accuracy cess later tokens, hindering relational knowledge
  - swer (e.g. Maria), which is also an entity. We find
  - hLT with this vector in Eq.2) (Nostalgebraist, 2020). play a significant role in activating important at-
  - its layer input hl鈭1 significantly enhances the log
  - conclusions in Geva et al. (2023), where FFN fea- mains low, even when both the first-hop and second-

## 6. Conclusion、局限与可复现性

- **结论段落线索**：tures are activated by the entity鈥檚 word embeddings hop queries are correct. In this section, we inves- and subsequently processed by attention layers. tigate the cause of this phenomenon. We focus on Additionally, we find that subject enrichment the prompt like 鈥渆1鈥檚 r1鈥檚 r2 is鈥, where the correct and attribute extraction occur not only at entity po- answer is 鈥渆3鈥. We use the logit flow method to sition but also at relation and last positions. Due analyze the 889 correct two-hop queries, as shown to the autoregressive nature of decoder-only LLMs, in appendix D. We find that the importance of at- the mechanisms at the entity position and r1/last po- tention neurons at relation positions is significantly sitions differ. At entity position, lower-layer FFN lower than that in single-hop queries. Based on this and attention neurons encode knowledge about 鈥渆1 observation, we hypothesize that the model may in- -> e1 features鈥. In contrast, at the relation and last correctly predict the enti
- **局限/未来工作线索**：2-layer transformer. When applied to pre-trained ploring the limitations of large language models on；8 Limitations Circuits Thread, 2.
- **可复现核对表**：模型与版本、数据集切分、prompt、随机种子、baseline 实现、评价脚本、解释单元位置、干预强度、显存/时间成本。

## 7. 一句话定位

这篇论文把“Back Attention: Understanding and Enhancing Multi-Hop Reasoning in Large Language Models”放在从行为现象/内部表征分析走向可验证解释、可控干预或可信应用的研究链条上；真正的贡献需要通过其 baseline、ablation 和跨设置 finding 共同判断。
