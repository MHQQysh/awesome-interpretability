# 037. Are self-explanations from Large Language Models faithful?

- **作者 / venue**：Lora A. A. et al.；Findings of ACL 2024
- **论文**：[ACL Anthology](https://aclanthology.org/2024.findings-acl.19/)
- **任务**：评估 LLM 自解释、feature attribution、redaction 与 counterfactual explanation 的 faithfulness

## 1. Introduction

LLM 能用自然语言解释自己为何做出一个分类或答案，但 fluent explanation 可能只是事后合理化。论文不把 explanation quality 等同于 accuracy，而是问：解释提到的特征是否真的影响模型预测？并强调 faithfulness 需要 out-of-domain evaluation，避免模型只记住某一类 explanation test。

## 2. Method

- 对不同模型和任务生成多种 explanation：free-text self-explanation、feature attribution、redaction/counterfactual。
- 通过修改解释中提到的输入特征，检查输出是否按解释预测改变。
- 使用 self-consistency checks 和 counterfactual prompts，测试 explanation 是否稳定。
- 任务覆盖 sentiment、classification 和其他自然语言决策。

## 3. Baseline 与对比

- LLM self-explanation。
- feature attribution。
- redaction explanation。
- counterfactual explanation。
- 不同模型：Llama2、Mistral、Falcon 40B 等；不同任务分别比较哪类解释更忠实。

## 4. Findings

- self-explanation faithfulness 高度依赖模型、数据集和任务，不能笼统地说 LLM 自解释可信。
- sentiment classification 中，Llama2 的 counterfactual 更忠实，Mistral 的 feature attribution 更好，Falcon 40B 的 redaction explanation 在部分设置更有优势。
- 一个模型在某任务上能生成 faithful explanation，不代表换任务后仍然 faithful。
- 解释是否提到了重要特征，与解释文本是否能让人感觉合理是两回事。

## 5. 方法学提醒

如果只删除解释中提到的 token，可能把“解释长度”误认为重要性；如果只看 label flip，又会忽略概率变化。还要防止把测试用的 counterfactual 当成训练分布内的提示模板。

**一句话评价**：论文最重要的结论是 self-explanation 没有普遍 faithfulness 保证，必须按模型、任务和干预协议逐项验证。
