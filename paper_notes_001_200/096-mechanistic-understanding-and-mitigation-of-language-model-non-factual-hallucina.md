# 096. Mechanistic Understanding and Mitigation of Language Model Non-Factual Hallucinations

> 人工精读笔记：Findings of EMNLP 2024。重点是把 hallucination 按内部 fact-recall pipeline 拆成两种机制，再做 targeted restoration。

## 0. 论文信息

- **作者**：Lei Yu, Meng Cao, Jackie Chi Kit Cheung, Yue Dong
- **来源**：[Findings of EMNLP 2024](https://aclanthology.org/2024.findings-emnlp.466)
- **代码/数据**：[lm_hallucination_mechanisms](https://github.com/jadeleiyu/lm_hallucination_mechanisms)
- **模型**：Llama-2、Pythia、GPT-J

## 1. Introduction：要解决什么问题

非事实 hallucination 可能在模型很自信时出现，外部 uncertainty 或 consistency 检测可以发现一部分风险，却解释不了错误在内部何处形成。若对所有 hallucination 统一做 decoding 控制或全模型 editing，容易损害正常事实能力；作者希望定位“错误类型对应的具体组件”，再只恢复受损的 fact recall path。

## 2. Method / Framework：怎么解决

作者从 ParaRel 构造 subject-relation-object cloze diagnostic dataset，将查询分成真实事实、知识错误和答案抽取错误。先用 logit lens 观察中间层逐步解码出的对象，再用 causal mediation/activation patching 测量每层 MLP 与 attention 对最终 hallucination 的 indirect effect。

### 2.1 两种机制

1. **Knowledge enrichment hallucination**：subject 本身较陌生，早期/中层 MLP 没有注入足够的 subject attribute knowledge，后续生成的答案常是 nonsensical 或泛化错误。
2. **Answer extraction hallucination**：模型已有相关对象知识，但中后层 self-attention 没有从 subject-relation 组合中选择正确 object，反而提取了与 subject 更强关联的错误属性。

### 2.2 Mechanistic Hallucination Mitigation

MHM 根据 hallucination query 与 factual counterpart 的 clean/corrupted activation，恢复与事实 recall pipeline 相关的隐藏表示或模块输出。与全模型 fine-tune 相比，它针对下层 MLP 或上层 attention 的受损路径做选择性 restoration，再在 open-domain QA 上评测 factuality 和 utility。

## 3. Baseline / 对比基线

- **SFT**：用事实数据 fine-tune，代表通用训练式 mitigation。
- **MEND**：模型 editing baseline。
- **DoLa**：利用不同层 logits 的 decoding mitigation。
- **原始 LM**：Llama-2/Pythia/GPT-J 的普通生成。
- **两类 hallucination 分组**：不是 baseline，而是最关键的机制对照，检验同一种恢复策略是否对两类错误都有效。

## 4. Experiments / Findings：结果如何读

logit lens 和 causal tracing 在三个模型上收敛到相同图景：knowledge enrichment 错误在早期/中层 MLP 就缺少有效 subject 信息；answer extraction 错误在中后层 attention 仍无法选中正确 object。两类错误的外部表现也不同：前者通常与未知 subject 相关，后者常是模型对错误 object 已有强关联。

在阈值较严格的 factual query 上，大多数 hallucination 由 answer extraction 主导；模型之间比例不同，GPT-J 的 enrichment hallucination 更多，说明机制分布会随模型/训练而变。因果 patching 显示，把 factual run 的相关中间状态写回 hallucination run 后，错误答案的 log-likelihood ratio 会向真实答案移动，支持定位不是纯相关性。

MHM 在知识问答上比 SFT、MEND、DoLa 等 baseline 更好地减少 hallucination，同时保持正常事实预测效用；表 2 的结果显示针对不同机制恢复相应模块优于不区分类型的统一 intervention。这里的主要贡献不是某个固定百分比，而是“类型诊断 -> 组件定位 -> 定向恢复”的可复用流程。

## 5. Ablation / 机制解释

- logit lens vs causal tracing：前者看表示中何时出现错误，后者检验改动该表示是否真的影响输出。
- 低层 MLP restoration vs 上层 attention restoration：分别对应 enrichment 与 extraction，验证机制-干预匹配。
- MHM vs SFT/MEND/DoLa：比较选择性内部恢复与通用 mitigation 的准确率-副作用权衡。
- 多模型 replication：Llama-2、Pythia、GPT-J 验证两类机制不是单一架构 artifact。

## 6. Limitations / 局限与复现注意

- ParaRel 是 cloze factual knowledge，不能覆盖长文本、对话、工具使用或多跳生成 hallucination。
- causal tracing 的 corruption、patch layer、目标 token 和归一化会影响组件排名。
- MHM 需要 factual counterpart 或可构造的 clean state，真实未知事实场景不一定可用。
- “减少 hallucination”与“模型真正知道事实”仍需外部知识和更广泛任务验证。

## 7. 两句话总结

论文把非事实 hallucination 分成下层 MLP 缺少 subject knowledge 的 enrichment error 和上层 attention 选错 object 的 extraction error。它用 logit lens 与 causal tracing 定位两条内部路径，再做类型匹配的 targeted restoration，整体优于 SFT、MEND 和 DoLa，但诊断数据主要是 cloze factual queries。
