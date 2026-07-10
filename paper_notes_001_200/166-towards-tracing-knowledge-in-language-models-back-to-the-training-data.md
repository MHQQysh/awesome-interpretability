# 166. Towards Tracing Factual Knowledge in Language Models Back to the Training Data

- **Authors:** Ekin Akyürek, Tolga Bolukbasi, Frederick Liu, Binbin Xiong, Ian Tenney, Jacob Andreas, Kelvin Guu
- **Venue:** EMNLP Findings 2022
- **Paper:** https://aclanthology.org/2022.findings-emnlp.180
- **Tags:** Data Attribution, Fact Tracing, Influence, Training Data

## Introduction

LM 生成一个事实时，很难知道它从哪条训练样本学到，也无法区分 literal memory 与间接推断。论文把问题定义为 fact tracing：找出真正教会模型某个 factual assertion 的 training examples，称为 proponents，并首次提供可量化 benchmark。

## Method / Framework

FTRACE-TREX 从 TREx 中找出模型原先不知道、再通过 fine-tuning 注入的事实，注入样本作为 ground-truth proponents；FTRACE-SYNTH 用合成事实控制注入和训练过程。模型给定 factual prompt，TDA 方法返回可能影响该预测的训练样本。

比较 gradient-based attribution（如 influence / TracIn 家族）与 embedding-based retrieval，并研究 gradient saturation、checkpoint、layer、normalization、sequence aggregation 等实现细节。

## Baselines / Comparisons

包括 BM25 information retrieval baseline（不访问 LM 也能检索）、gradient-based TDA、embedding-based TDA 和不同实现 variant。指标是 proponent retrieval precision / recall、top-k 命中和跨事实 / 跨模型泛化。

## Experiments / Findings

- gradient / embedding TDA 在真实和合成 fact tracing 上都有信号，但两者的 proponent precision 都低于 BM25 baseline，说明当前 attribution 不能可靠指出“真正教会模型的句子”。
- gradient saturation 是主要障碍：模型学会事实后 loss 梯度变小，真正的注入样本反而难以被 influence 找回。
- checkpoint、gradient averaging、target token 选择和 query / training example 表示方式等细节会显著改变结果；改进实现后可以提高但仍有很大 headroom。
- literal proponents 比 indirect proponents 更容易追踪；模型可能从 “Obama born in Honolulu” 间接得到 “Obama born in Hawaii”，而 benchmark 只给前者作为 ground truth 的限制。

## Ablation / Error Analysis

消融 gradient layer、训练 checkpoint、target token、embedding pooling、BM25 文本化方式和事实定义。错误包括训练样本语义相似但不是 causal source、事实间逻辑蕴含、多条重复网页和模型已经在 pretraining 中知道事实。随机 / IR baseline 强说明“相似文本”不等于“影响预测”。

## Limitations

FTRACE 的 ground truth 主要通过 controlled injection 构造，不能完全反映真实预训练。只识别 literal proponents，不能覆盖间接知识链；大规模语料的精确 TDA 仍计算昂贵。retrieval precision 低也可能来自多个真实 proponents，不能简单把 top-1 失败当作方法完全无效。

## 两句话总结

FTRACE 把“模型知道事实”推进为“哪条训练样本教会了它”，并发现现有 gradient / embedding attribution 甚至不如 BM25 稳定。结果说明训练数据追踪必须处理梯度饱和、间接知识和多源重复，不能把语义相似检索当成因果归因。
