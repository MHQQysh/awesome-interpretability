# 130. Sources of Hallucination by Large Language Models on Inference Tasks

> 人工精读笔记：Findings of EMNLP 2023。重点是 inference 任务中的 attestation bias 与 relative frequency bias，以及实体作为记忆索引的作用。

## 0. 论文信息

- **作者**：Nick McKenna, Tianyi Li, Liang Cheng, Mohammad Javad Hosseini 等
- **来源**：[Findings of EMNLP 2023](https://aclanthology.org/2023.findings-emnlp.182)
- **任务/模型**：Levy/Holt、RTE-1；LLaMA、GPT-3.5、PaLM 等 NLI/推理模型

## 1. Introduction：要解决什么问题

NLI 应只依据 premise 判断 hypothesis 的 entailment，但 LLM 可能使用预训练记忆和语料频率，产生与 premise 无关的 false positive entailment。论文把 hallucination 追溯到两种可控 behavioral bias，而不是把所有错误归因于推理能力不足。

## 2. Method / Framework：怎么解决

### Attestation bias

比较模型是否在预训练语料中见过 hypothesis（attested）。把 premise 随机化，使其不再 entail hypothesis；若模型仍因 hypothesis 熟悉而预测 entail，就构成 false positive hallucination。

### Relative frequency bias

控制 premise/hypothesis 中实体或 predicate 的语料频率，构造频率高低但 entailment label 不变的样本；观察模型是否偏向更常见的表述。

### Entity replacement

将原实体替换为 generic argument、低频实体或高频实体，同时保持类型/逻辑标签，测试实体是否作为 propositional memory 的索引。

## 3. Baseline / 对比基线

标准 NLI、hypothesis-only baseline、random-premise、generic/frequent/infrequent entity replacement、attested vs unattested subset、LLM zero/few-shot prompt。

## 4. Experiments / Findings：结果如何读

在 random premise 条件下，若 hypothesis 被模型判为 attested，LLaMA/GPT-3.5/PaLM 预测 false entail 的概率分别约为普通条件的 1.9x/2.2x/2.0x，说明模型把 prior memory 混进了 premise inference。该效应跨预训练模型存在，不能只归因于 instruction tuning。

实体替换使 recall 显著下降，尤其高频实体替换；GPT-3.5 recall 可从原始约 92.3 降到高频随机实体约 55.3。低频实体比高频实体更能保持 novel inference，说明实体是调用 propositional memory 的索引。

相对频率 bias 也会让无语义关系的样本更容易被预测为 entail；在屏蔽实体记忆后，频率效应反而更明显，揭示 memory bias 与 predicate pairing bias 之间的张力。与 bias 一致的子集性能远高于 adversarial 子集，说明总体 NLI 分数掩盖了机制性脆弱点。

## 5. Ablation / 机制解释

random premise、entity masking、frequency conditioning、consistent/adversarial split 和多模型 replication 逐步排除表面相关。实验支持两条 hallucination source：预训练 proposition memory 和语料相对频率，而不是单一“模型不懂逻辑”。

## 6. Limitations / 局限与复现注意

attestedness 和 corpus frequency 需要估计，无法完全观察；NLI 模板化，不能覆盖真实长文本推理；prompt/template 和 confidence threshold 会影响结果；减少 memory bias 可能暴露其他 shortcut。

## 7. 两句话总结

论文发现 LLM 在 NLI 中会把 hypothesis 的预训练记忆和实体/词频先验带入本应只依赖 premise 的推理，造成 false entailment hallucination。随机 premise、实体替换和频率控制实验显示，记忆调用与相对频率是两条可区分的错误来源，且会显著拉大 consistent/adversarial 性能差距。
