# 440. Neural Authorship Attribution: Stylometric Analysis on Large Language Models

- **Authors:** Tharindu Kumarage, Huan Liu
- **Venue / year:** CyberC 2023, pp. 51-54
- **Paper:** https://arxiv.org/abs/2308.07305
- **Tags:** `stylometry` `authorship-attribution` `AI-forensics` `SHAP` `interpretability`

## Introduction
生成式模型被用于新闻、影响行动和虚假信息时，取证人员不仅需要判断文本是不是 AI 写的，还希望追溯它来自哪个 LLM。本文把 neural authorship attribution 细分为 proprietary vs open-source 的初始归因，以及同一类别内部的模型归因，并用可读的 stylometric features 解释模型之间留下的 writing signatures。

## Method / Framework
作者从 CNN 和 Washington Post 新闻语料抽取 1,000 个 headline，把每个 headline 交给 GPT-4、GPT-3.5、Llama 1、Llama 2 和 GPT-NeoX 生成文章，得到 6 个模型各 1,000 篇、共 6K 篇文本；闭源模型使用 `gpt-4-0613` 和 `gpt-3.5-turbo-0613`，生成参数为 top-p 0.9、temperature 0.8。

每篇文本提取 60 个写作特征：lexical 包括平均词长、function-word、MTTR、hapax legomena 和 stopword；syntactic 包括句长、POS、主动/被动语态和时态；structural 包括段落长度、标点频率和大写使用。比较 XGBoost+Bag-of-Words、XGBoost+stylometry、zero-shot/fine-tuned RoBERTa，并提出 RoBERTaStylo，将 RoBERTa embedding 与 stylometry 表示经 attention 融合后分类；再用 SHAP 分析特征重要性。

## Baselines / Comparisons
任务分为三种：proprietary/open-source 二分类、GPT-3.5 vs GPT-4 的闭源内部归因、GPT-NeoX/Llama1/Llama2 的开源内部归因。XGBBOW 是词袋基线，XGBStylo 是可解释特征基线，RoBERTaZero 与 RoBERTaFT 代表预训练模型和微调模型，RoBERTaStylo 检查可解释 stylometry 是否能补充 PLM 语义表示。

## Experiments / Findings
表 I 的 F-score 显示，初始 proprietary/open-source 归因中 XGBBOW/XGBStylo/RoBERTaZero/RoBERTaFT/RoBERTaStylo 分别为 0.858/0.845、0.954/0.949、0.881/0.873、0.972/0.968、0.992/0.987；融合模型效果最好。闭源内部归因中 RoBERTaStylo 对 GPT-3.5/GPT-4 达到 0.947/0.951，开源内部归因对 GPT-NeoX/Llama1/Llama2 为 0.813/0.802/0.917。

t-SNE 显示两类模型在 embedding 和 stylometry 空间都可分，但 stylometry 的类别分离更明显；GPT-3.5 与 GPT-4 仍有重叠，提示同源模型风格趋同但并不完全相同。Llama1 与 GPT-NeoX 重叠较多，而 Llama2 更接近 proprietary 模型；SHAP 指出词汇多样性、POS（尤其介词/形容词/名词）、标点和段落结构是区分类别的重要信号。

## Ablation / Error Analysis
从 RoBERTa 到 RoBERTaStylo 的提升说明语义 embedding 与风格统计是互补的，而单纯 BoW 不能捕捉足够稳定的模型签名。只用 Llama2 代表 open-source 类别时，初始归因性能平均下降 7.4%，说明“open-source”内部并非同质类别，模型能力和训练流程变化会改变风格。

错误主要来自类别内部风格重叠：GPT-NeoX/Llama1 写作分布相近，GPT-3.5/GPT-4 也可能共享预训练或组织层面的特征。SHAP 提供了可解释线索，但这些特征是相关性证据，不能证明某个 POS 或标点是模型来源的因果指纹。

## Limitations
研究只覆盖六个模型、新闻文章和固定 headline 提示；不同 prompt、解码参数、领域和人工后编辑都可能改变 stylometry。论文将 attribution 当作闭集分类，面对未知模型、混合生成、翻译或改写文本时泛化未知；数据规模虽为 6K，但每个模型只有 1K，且 proprietary API 版本会随时间变化。

## 两句话总结
本文把 LLM 来源归因变成可解释的写作风格分类，证明 60 个词汇、句法和结构特征能补充 RoBERTa embedding，并用 SHAP 指出 lexical diversity、POS、标点和段落结构是关键信号。融合模型在 proprietary/open-source 初始归因上接近完美，但模型内部风格重叠、prompt/解码敏感性和未知模型开放集问题仍是主要风险。

## Evidence note
已从本地下载并逐段读取 arXiv PDF，核对了 6K 新闻数据、60 个 stylometric features、XGB/RoBERTa/RoBERTaStylo 对比、Table I F-score、t-SNE、SHAP 和 7.4% 降幅分析。
