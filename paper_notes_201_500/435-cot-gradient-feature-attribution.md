# 435. Analyzing Chain-of-Thought Prompting in Large Language Models via Gradient-based Feature Attributions

- **Authors:** Skyler Wu, Eric Shen, Charumathi Badrinath, Jiaqi Ma, Hima Lakkaraju
- **Venue / year:** ICML Workshop on Challenges in Deployable Generative AI, 2023
- **Paper:** https://arxiv.org/abs/2307.13339
- **Tags:** `chain-of-thought` `feature-attribution` `saliency` `faithfulness` `gradient`

## Introduction
CoT 能在大模型上提高问答准确率，但“生成了 reasoning”不等于模型真的按这段 reasoning 进行计算。本文不直接把自然语言 rationale 当解释，而是问 CoT 是否改变了模型对输入 token 的内部敏感度，以及这种敏感度是否更稳定。作者选择梯度特征归因作为相对输出语义更中立的窗口，研究 CoT 相比 standard few-shot prompting 是否更关注问题中的语义相关 token。

## Method / Framework
实验对同一组 few-shot examples 构造 standard prompt（直接给答案）和 CoT prompt（先解释再给答案），对 GPT-J 6B 主结果，并在 GPT-Neo 2.7B、GPT-2 XL 1.5B 上复现趋势。对最终答案 token，计算问题 token 和生成答案前缀 token 的四种 saliency：regular/contrastive 两种 gradient×input 与 L1 norm 方法。作者人工标注每个问题中语义相关 token，再比较其 saliency 的绝对幅度、改写鲁棒性和随机生成鲁棒性。

数据是四个各 50 题的小规模集合：SST、CoinFlip、GSM8K、CommonsenseQA；另外把 CoinFlip/GSM8K 手工改写成语义等价但句法不同的版本。第三组实验对每题重复生成 20 次，测试答案和 CoT 变化时相关 token 的梯度是否仍稳定。

## Baselines / Comparisons
直接比较 standard few-shot 与 CoT few-shot，而不是把 CoT 与不同模型能力混在一起；模型规模、提示示例和问题尽量固定。四种 attribution 方法互相对照，三个模型规模做稳健性检查，实验维度包括原题 vs reworded、确定性/采样生成和最终准确率。准确率结果也作为行为层面的基线，检验 saliency 变化是否只是因为 CoT 改善了任务表现。

## Experiments / Findings
在 GPT-J 6B 上，CoT 并没有普遍提高准确率，SST 是明显例外，accuracy 从 0.50 提升到 0.64；这与 CoT 通常需要更大模型才显著提升任务准确率的结论一致。更值得注意的是，CoT 在四个数据集和四种归因方法上都降低了语义相关问题 token 的 saliency magnitude，而不是简单让模型给这些 token 更高权重，作者解释为更长的 rationale 使梯度分布到更多 token。

CoT 的主要内部变化是稳定性而非幅度：对 CoinFlip/GSM8K 的语义等价改写，CoT 下相关 token 的 saliency 在原题和改写题之间变化更小；对 20 次随机生成，CoT 形成更紧的 saliency cluster，即便 CoT 文本和最终答案有更多变化。例如 GPT-J 的选定 GSM8K 问题，CoT 产生 18 种 unique answer/rationale 组合，standard 只有 6 种，但相关 token 的梯度反而更集中。

## Ablation / Error Analysis
四种 saliency 公式并不完全一致，绝对值尺度也不同；但“改写后更稳定”和“重复生成更稳定”的方向在 GPT-J 上较一致，在 GPT-Neo/GPT-2 XL 上总体复现但个别数据集较弱。作者指出 GPT-Neo 的 CoinFlip/GSM8K 仍有清晰趋势，而某些任务的差异不如 GPT-J 明显；这意味着结论更适合表述为归因稳定性证据，而不是 CoT 已经被证明为 faithful reasoning。

误差还来自人工相关 token 标注的主观性、模型可能输出格式异常（CSQA 选项没有括号、SST 偶尔输出未提供的 neutral），以及在 1.5B–6B 模型上研究与大模型 CoT 行为的尺度差异。更长 CoT 的 saliency dilution 也可能让幅度比较难以直接解释。

## Limitations
模型最多 6B，正是 CoT 任务收益尚未普遍出现的规模；数据集每个只有 50 题，相关 token 由作者手标。梯度归因方法本身存在方法间 disagreement，且需要白盒梯度访问，不能直接用于闭源模型；论文没有证明 saliency 稳定性必然等于真实因果使用了这些 token。

## 两句话总结
本文用四种梯度归因方法比较 standard prompting 与 CoT，发现 CoT 不会普遍放大相关 token 的 saliency，却会让这种 saliency 对问题改写和随机输出更稳定。这个结果把 CoT 的一个可能机制从“看起来更像推理”推进到“内部输入敏感度更稳”，但小模型、小样本和归因方法自身的不确定性仍限制了结论。

## Evidence note
已逐段读取本地 PDF，核对了 GPT-J/GPT-Neo/GPT-2 XL、SST/CoinFlip/GSM8K/CSQA、四种 saliency、三组实验、SST 0.50→0.64 及 20 次重采样分析。
