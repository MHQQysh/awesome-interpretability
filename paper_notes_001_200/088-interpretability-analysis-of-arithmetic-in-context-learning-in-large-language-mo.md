# 088. Interpretability Analysis of Arithmetic In-Context Learning in Large Language Models

> 人工精读笔记：EMNLP 2025。重点是区分 arithmetic symbol information 与 in-context example 的 pattern information。

## 0. 论文信息

- **作者**：Gregory Polyakov, Christian Hepting, Carsten Eickhoff, Seyed Ali Bahrainian
- **来源**：[EMNLP 2025](https://aclanthology.org/2025.emnlp-main.92)
- **代码**：[arithmetic-icl-interpretability](https://github.com/ali-bahrainian/arithmetic-icl-interpretability)
- **模型**：Pythia-12B、OPT-6.7B、MPT-7B、Llama-3.1-8B

## 1. Introduction：要解决什么问题

LLM 在加入一个 in-context example（ICE）后，三操作数 arithmetic 的准确率显著提高，但这种提升来自真正学会了算术，还是只识别了示例的格式和结果模式？已有 mechanistic interpretability 研究主要分析两操作数或简单 function vector，尚未解释复杂三操作数任务中 ICE 的 symbol-level 和 pattern-level 信息如何流动。

论文把 ICE 拆成两类因素：symbol information 是具体 operand、operator 和算术正确性；pattern information 是 token 顺序、等号结构、数字/英文格式和结果位置的一致性。作者用 activation patching、information flow routes、function vectors 和 edge attribution patching 逐步定位信息路径。

## 2. Method / Framework：怎么解决

### 2.1 数据与基线

Arithmetic-20 让 0--20 的整数尽量使用单 token，包含加减乘和数字/英文两种形式。选取模型在 one-shot ICE 中答对、zero-shot 中答错的样本，以隔离 ICE 的作用。Pythia-12B 的 one-shot accuracy 从 zero-shot 28.3% 提高到 52.1%。

Arithmetic-1000 扩展到更大的整数，Llama-3.1-8B 的 zero-shot accuracy 为 30.0%，one-shot 提升到 76.7%；zero-shot prompt 会 padding 到与 one-shot 长度一致，避免长度造成 patching 偏差。

### 2.2 Activation patching

记录含 ICE 的 clean prompt activation，再把 ICE 删除的 corrupted prompt 中某个 token/layer 的 attention、MLP 或 residual activation 替换回来。以正确答案和错误预测的 logit difference 变化定义 patching effect，定位哪些 token 和模块对成功解题有因果贡献。

### 2.3 Information flow / function vector

Information Flow Routes 从最终预测向后追踪重要组件；function vector 则从 ICE 结果位置相关的 attention heads 中提取平均向量，注入 zero-shot prompt，检验是否能压缩传递 ICE 的模式信息。Edge Attribution Patching 进一步找出 Pythia-12B 的算术子电路。

## 3. Baseline / 对比基线

- **Zero-shot p0 vs one-shot p1**：最基本性能基线，确定 ICE 是否真正帮助模型。
- **Arithmetically correct/incorrect result replacement**：只改 ICE result 的 symbol，测试算术正确性。
- **Format-consistent/inconsistent replacement**：保持或破坏数字/英文格式，测试 pattern。
- **Random non-numeral replacement**：控制 token identity 被完全破坏时的影响。
- **Operand replacement**：改 operand，比较具体数字与 result token 的因果重要性。
- **多模型 replication**：Pythia、OPT、MPT、Llama 对照，避免发现只是某个架构的 artifact。

## 4. Experiments / Findings：结果如何读

### 4.1 结果 token 比具体算术更重要

表 1 的 Pythia-12B 结果：基线 p1 为 100%；把 ICE result 换成算术错误但格式一致的 token，准确率降到 37.4%；换成格式不一致的 result，降到 30.6%；同时错误且格式不一致约 29.2%，换成随机非数字符号则降到 0%。

只替换 ICE operands 的准确率仍为 69.4%，说明模型并非完全依赖每个具体数字。pattern-consistent 的错误结果仍保留相当能力，表明“这是一个合法 ICE result、位置和格式正确”比具体 symbol correctness 更能驱动后续任务。

### 4.2 信息流的阶段性

早期 MLP/attention 处理 ICE token；attention 把 ICE result 的模式信息聚合到 result/equals 位置；中层再把该信号路由到 task equals sign；最终层组合任务上下文与 result-derived pattern。Logit lens 显示部分算术和只在晚层出现，且不是中层驱动正确答案的主要信号。

在 Pythia-12B 中，EAP 得到的完整 arithmetic circuit 与只处理 ICE result 的 circuit 有约 56% edge overlap，而 operand/operator 专属 circuit 的 overlap 不超过 22%，进一步支持 result token 的中心作用。

### 4.3 Function vector 与跨模型

从 ICE 相关 heads 提取的 function vector 可以注入 zero-shot prompt，使其准确率接近 one-shot，而不需要显式保留全部 ICE。Llama-3.1-8B 在大数 Arithmetic-1000 上也出现类似提升，说明向量携带的是可复用的任务模式，但不同模型的峰值 layer 不完全一致。

MPT-7B 的 one-shot accuracy 从 6% 到 43%；其 patching 峰值更偏向第二个 MLP layer，说明架构（例如 ALiBi）会改变信息传递位置，但总体 pattern-level finding 相近。

## 5. Ablation / 机制解释

- Exp. 3--6 分离 arithmetic correctness、format consistency、symbol identity 和 operand content。
- Exp. 7 联合 patch 第一层 ICE-result MLP 与后续 attention，检验早期写入和后续路由的交互。
- residual stream、MLP、attention、logit lens、information flow 和 EAP 交叉验证，避免单一 attribution 方法的假阳性。
- 加入更详细自然语言 task description 后，patching pattern 基本保持，说明发现不只依赖极简 prompt。

## 6. Limitations / 局限与复现注意

- Arithmetic-20 为单 token 数字设计，真实数学任务和多 token 数字可能有不同 circuit。
- Information Flow Routes 受 TransformerLens/模型架构兼容性限制，MPT 等模型部分分析只能用 patching。
- “pattern 更重要”不等于模型完全不会算术；晚层仍出现 arithmetic representation，论文结论是它不是主要驱动路径。
- function vector 依赖数据平均和 head 选择，跨任务、跨语言、跨复杂度的可迁移性需要更多验证。

## 7. 两句话总结

论文发现三操作数 arithmetic ICL 主要先把 ICE 的格式和结果位置模式传到任务位置，而不是在中层直接复制每个具体数字的算术内容。activation patching 和 EAP 共同定位了早期 result processing 与后续 attention routing，并表明抽取出的 function vector 能把 one-shot 模式压缩到 zero-shot 推理中，但复杂数字与架构变化仍会改变具体路径。
