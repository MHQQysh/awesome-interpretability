# 075. Causal Interventions Expose Implicit Situation Models for Commonsense Language Understanding

- **作者 / venue**：Findings of ACL 2023
- **论文**：[ACL Anthology](https://aclanthology.org/2023.findings-acl.839/)

## Introduction / Method

commonsense language understanding 常被解释为模型记住词共现，但人类会构建 situation model。论文通过 context cue manipulation 和 causal intervention，比较模型在语义等价句子中对正确/错误 referent 的概率变化，检验是否存在隐式世界状态。

## Baseline 与对比

比较自然 context、替换 pronoun/verb 的 counterfactual context、token-specific controls 和语义同义词 controls。核心是排除模型只依赖表面 token 或位置。

## Findings

干预 context 会有方向性地改变 referent probability，说明模型内部状态包含超出局部词共现的 situation information。结果支持一些 commonsense 关系在模型中可被因果激活，但不同句式和 lexical realization 的稳定性有限。

## 局限

因果 prompt 只覆盖有限情境；改变一个 token 可能同时改变语法和语义。需要更大范围、多语言和 path-level intervention。

**一句话评价**：论文用 counterfactual causal tests 说明 Transformer 可能构建隐式 situation model，而不是只做静态词匹配。
