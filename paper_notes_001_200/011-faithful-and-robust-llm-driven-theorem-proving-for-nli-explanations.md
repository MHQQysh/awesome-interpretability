# 011. Faithful and Robust LLM-Driven Theorem Proving for NLI Explanations

- **作者 / venue**：Xin Quan, Marco Valentino, Louise A. Dennis, Andre Freitas；ACL 2025
- **论文**：[ACL Anthology](https://aclanthology.org/2025.acl-long.867/)
- **任务**：NLI explanation refinement；把自然语言解释转成形式逻辑并用 theorem prover 验证

## 1. Introduction：论文到底在解决什么

NLI 的解释不是简单地“给出一个看起来合理的理由”，而是要说明 premise 为什么蕴含 hypothesis。以往 LLM 生成的解释容易出现三类问题：自然语言到逻辑形式的自动形式化会丢失语义；形式逻辑中会出现语法错误或内部矛盾；即使 theorem prover 给出反馈，LLM 也不一定能把反馈整合成一个完整、忠实的证明。

论文的研究缺口因此不是“能不能生成解释”，而是**解释是否保留原始语义、是否能通过形式验证、以及能否在验证反馈下稳定修正**。作者将已有的 Explanation-Refiner 作为主要基线，并把 Faithful-Refiner 定位为一个 LLM + theorem prover 的联合 refinement 框架。

## 2. Method：四个环节如何串起来

1. **更稳健的 autoformalisation**：用 Neo-Davidsonian event semantics 将自然语言事件、参与者和关系转成逻辑表示，减少复杂句子的语义丢失。
2. **语法错误检查与修复**：在送入 theorem prover 前检查逻辑表达式；对变量、量词和关系连接进行显式修正，而不是让 prover 直接吞下错误形式。
3. **逻辑一致性 refinement**：用 SymPy 等工具推导逻辑蕴含、检查公理之间的矛盾，并把结构化反馈提供给 LLM。
4. **proof sketch 引导**：让 LLM 先生成结构化的证明草图，再由 prover 搜索和验证证明；如果失败，则以失败类型为条件进行迭代修复。

这个设计的关键不是单独使用 theorem prover，而是把 prover 放在生成循环中：LLM 负责语言理解和候选证明，符号工具负责硬约束检查，LLM 再根据反馈修正解释。

## 3. Baseline 与对比协议

- **主要 baseline**：Explanation-Refiner，代表已有的 LLM-driven NLI explanation refinement。
- **模型维度**：论文比较 GPT-4o、GPT-4o-mini、Llama 3.1 70B、DeepSeek-V3 等不同 LLM，避免把结论绑定到一个模型。
- **数据集**：e-SNLI、QASC、WorldTree；从单步自然语言推理到多跳、知识密集型解释，难度逐步增加。
- **指标**：autoformalisation 的有效率/faithfulness、解释 refinement 成功率、逻辑有效性、平均 refinement 迭代次数，以及人工评价。
- **为什么这些 baseline 合理**：只比较最终 NLI accuracy 会掩盖解释错误；Explanation-Refiner 提供同类系统对照，不同 LLM 则用于检验框架是否依赖某个模型的语言能力。

## 4. Findings：结果说明了什么

- 相对 state-of-the-art baseline，autoformalisation 在三个数据集上的提升分别达到 **18.46%、34.2%、39.77%**；解释 refinement 提升分别为 **29.5%、51.5%、41.25%**。
- 复杂数据集上的收益更能说明问题：当解释包含多跳关系或多个公理时，单纯生成式 refinement 更容易出现语义漂移，而语法检查、逻辑一致性和 proof sketch 提供了逐步约束。
- 论文报告了不同 refinement 阶段的曲线：随着修正迭代进行，逻辑有效解释数量增加，内部 syntax error 减少；但继续迭代也可能带来额外成本，因此“越多轮越好”并不成立。
- 运行成本是代价。相比 Explanation-Refiner，Faithful-Refiner 需要多次 LLM 调用和 theorem prover 检查；论文因此同时比较 refinement 次数、时间和成本，而不是只报告准确率。

## 5. Ablation：去掉模块会发生什么

- 去掉 syntactic parsing 后，复杂句子的形式化更容易出现结构错误，后续 prover 失败率上升。
- 去掉 quantifier refinement 会削弱对全称/存在量词的处理，尤其影响多实体、多关系的 NLI 解释。
- 去掉 logical consistency check，系统可能保留可以形式化但互相矛盾的公理；这类错误在表面语言评价中不一定暴露。
- 去掉 proof sketch 或外部反馈后，LLM 更难把多个逻辑条件组合成一个可验证证明。

这些 ablation 支持的不是“每个组件都独立带来同样的收益”，而是说明系统的收益来自**语言生成、符号检查、反馈修复三者的闭环**。

## 6. 局限与复现重点

- theorem prover 的语言覆盖和 formalisation 设计会限制可处理的 NLI 类型。
- prover 通过不等于自然语言解释完全忠实；形式化本身仍可能把原句翻译错。
- 需要固定 LLM、prompt、迭代上限、proof timeout 和错误修复策略，否则成本与成功率不可比。

**一句话评价**：这篇论文把“解释是否合理”转成了“解释能否被形式化、验证、反馈修复”的工程闭环，是自然语言解释 faithfulness 走向可验证推理的重要一步。
