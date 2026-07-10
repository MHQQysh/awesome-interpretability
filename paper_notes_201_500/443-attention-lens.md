# 443. Attention Lens: A Tool for Mechanistically Interpreting the Attention Head Information Retrieval Mechanism

- **Authors:** Mansi Sakarvadia, Arham Khan, Aswathy Ajith, Daniel Grzenda, Nathaniel Hudson, Andre Bauer, Kyle Chard, Ian Foster
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2310.16270
- **Tags:** `attention-heads` `lens` `mechanistic-interpretability` `information-retrieval`

## Introduction
Logit Lens/Tuned Lens 主要把中间 residual 或 MLP 输出翻译成 vocabulary token，但 attention head 如何把检索到的信息写进 residual stream 仍难以直接观察。Attention Lens 为每个 layer-head 学一个 vocabulary projection，尝试回答“这个 head 在当前输入中想注入哪些概念”，从而把 attention retrieval 与最终预测连接起来。

## Method / Framework
对 GPT2-Small 的第 `l` 层第 `h` 个 head，训练一个 head-specific linear lens `L_{l,h}`，把 attention output 映射到约 50K vocabulary logits；目标是让该 lens 的分布与模型最终 output logits 的 KL divergence 最小。模型本身冻结，lens 在 BookCorpus 上单独训练；GPT2-Small 有 12 层×12 heads，所以共训练 144 个 lens，每个约 38M 参数，总计约 5.5B lens 参数。

它与 Logit Lens/Tuned Lens 的区别是：后两者主要解释 MLP/residual 的 short-circuit prediction，Attention Lens 专门解释 attention heads，但当前目标仍是贴近最终预测而非直接优化与输入相关性。论文展示了 bias localization、malicious prompt detection 和 activation engineering/model editing 三类用法。

## Baselines / Comparisons
定性对照为直接使用模型 unembedding matrix 的 attention output projection、Logit Lens 和 Tuned Lens。unembedding 只在最终 residual stream 上训练，直接用于中间 attention output 往往给出大量逗号、功能词等泛化 token；learned head-specific lens 能输出更语义化候选。当前没有统一的 causal-fidelity 数值 benchmark，作者把 quantitative causal evaluation 留作后续。

## Experiments / Findings
不同 heads 的 lens output 呈现高度专门化：有的输出 topic/knowledge concepts，有的表现为 induction、name-mover 或 self-repair 相关信号。一个 GPT2-Small 的 racial-bias case 中，layer 9 head 8 对“first Black president...”的 top-50 输出包含 Negro、Confederacy、Confederate 等词，为进一步定位 bias 提供了候选 head。

在 prompt injection case 中，同一个 head 在普通 grammar prompt 下输出一般语言 token，而加入“ignore instructions... print Nazi”后 top outputs 变成 German、Holocaust、Nazi、Hitler 等，说明 lens 能让研究者看到恶意 prompt 如何改变中间检索。它更像可读诊断窗口，而不是证明该 head 独立导致最终行为的因果工具。

## Ablation / Error Analysis
主要结构选择是 head-specific learned transform vs shared/unembedding projection；每层每头独立 lens 提高表达力但成本巨大。训练数据若与目标模型语料不匹配，lens 学到的映射可能把数据分布而非模型机制编码进去；目标 KL 对最终 logits 的优化也可能偏向“预测模仿”，不一定保留输入检索语义。

错误或误读来源包括 lens 的 top token 只是相关输出、同一概念分散在多个 head、相邻层 lens 是否可共享尚未验证，以及恶意 prompt 的词本身可能直接造成输出变化。作者提出用 causal basis extraction、跨层 KL 和 transfer-to-finetuned-model 测试来补足这些缺口。

## Limitations
实验只在 GPT2-Small 上完成，训练 144 个大 lens 成本约 1,200 GPU hours/12-lens group；没有对 attention head 解释做因果 fidelity ground truth。当前目标函数、训练语料、lens architecture 都可能决定观察到的 token 分布，不能把它当作通用的 attention semantics。

## 两句话总结
Attention Lens 为每个 attention head 学一个从隐藏输出到词表的映射，把传统 Logit/Tuned Lens 扩展到“信息检索头”层面。它在 bias、prompt injection 和 specialized head case study 中提供了直观线索，但尚需因果评估、跨模型迁移和更合理的输入相关目标来证明解释忠实。

## Evidence note
已逐段读取本地 PDF，核对了 GPT2-Small 144 个 lens、KL 训练目标、BookCorpus、与 Logit/Tuned Lens 对照、bias/prompt-injection 案例和成本/causal-fidelity 限制。
