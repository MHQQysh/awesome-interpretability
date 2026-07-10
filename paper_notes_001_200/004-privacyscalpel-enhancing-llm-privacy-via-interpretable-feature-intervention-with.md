# 004. PrivacyScalpel: Enhancing LLM Privacy via Interpretable Feature Intervention with Sparse Autoencoders

## 4. PrivacyScalpel: Enhancing LLM Privacy via Interpretable Feature Intervention with Sparse Autoencoders

- **作者 / 发表**：Ahmed Frikha, Muhammad Reza Ar Razi, Krishna Kanth Nakka, Ricardo Mendes, Xue Jiang, Xuebing Zhou；BlackboxNLP 2025
- **论文链接**：[ACL Anthology](https://aclanthology.org/2025.blackboxnlp-1.13/)
- **方向**：SAE、隐私泄漏、PII 特征干预
- **要解决的问题**：LLM 可能记忆并泄漏个人身份信息；差分隐私或神经元级删除通常会损伤模型效用，或者因为神经元承载多个语义而不能稳定阻断泄漏。
- **怎么解决**：PrivacyScalpel 分三步工作：先用 feature probing 找到编码 PII 的层，再用 k-SAE 分离隐私相关稀疏特征，最后通过 targeted ablation 和 vector steering 在 feature level 进行干预。论文在 Gemma2-2B、Llama2-7B 和 Enron 数据上比较隐私与任务效用。
- **摘要概括**：在实验设置中，方法把 email leakage 从 5.15% 降到 0%，同时保留超过 99.4% 的原始效用，并优于神经元级方法。论文还把隐私防护转化为对 PII 记忆机制的解释性分析。
- **阅读要点**：这里的关键转变是把“解释模型”与“修模型”合并：先找可命名的稀疏特征，再删除或转向这些特征。需要注意的是，隐私比例和效用结果依赖数据、攻击方式与评测协议，不能直接外推到所有模型。
