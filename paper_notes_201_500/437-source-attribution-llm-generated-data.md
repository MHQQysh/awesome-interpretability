# 437. Source Attribution for Large Language Model-Generated Data

- **Authors:** Jingtan Wang, Xinyang Lu, Zitong Zhao, Zhongxiang Dai, See-Kiong Ng, Bryan Kian Hsiang Low, Chuan-Sheng Foo
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2310.00646
- **Tags:** `data-provenance` `source-attribution` `watermark` `WASA` `interpretability`

## Introduction
LLM 生成文本可能使用了某个数据提供者的受版权保护内容，但生成结果通常没有可验证来源。本文把问题从二元的“某数据是否被用于训练”推进到更细的 source attribution：给定生成句子，识别对它贡献最大的 data provider。这个任务同时服务 IP 保护、数据 provenance 和可解释性，但要求准确、抗攻击、可扩展、保持生成质量，而且水印应能跨模型训练传递。

## Method / Framework
WASA（WAtermarking for Source Attribution）为每个 data provider 分配一个由不可见 Unicode 字符组成的唯一 watermark。对每个 provider，先用 TF-IDF 选最能代表其风格的 top 20% 句子嵌入水印，再进行两阶段训练，使 WASA-LLM 学会从文本特征映射到 provider watermark，并在生成文本时在专门的 watermark token 空间生成水印。推理时解码水印即可得到 source。

作者将文本 token generation 与 watermark prediction 分离，以降低预测复杂度并显式强制生成水印；如果评测时句子过短没有自然生成 watermark，会追加 `[WTM]` 作为统一的测量机制。top-k 版本允许保留多个候选 source，适合一个句子混合多个提供者的情况。

## Baselines / Comparisons
主要基线是 BM25：用生成文本与各 provider 文本的 lexical matching 进行 source attribution。实验在 ArXiv 与 BookSum 数据集、GPT-2 与 Llama2 上报告 top-1/top-3/top-5 accuracy 和 macro F1；另测 watermark removal/modification、插入、删除、同义词替换、句法变换、provider 数量扩展、transferability 和文本生成质量。

## Experiments / Findings
10 个 provider 时，GPT-2 在 ArXiv 上 BM25 的 top-1/top-3/top-5 为 54.73/85.13/93.80，WASA 为 74.84/95.76/98.56，F1 从 0.517 升到 0.758；Llama2 WASA 为 77.40/96.87/99.40，F1 0.800。BookSum 上 GPT-2 WASA 为 77.92/91.80/96.52，Llama2 WASA 为 83.27/95.27/97.67，均明显高于 BM25。

去除或改动水印后，WASA-LLM 可根据文本重新生成 watermark：GPT-2/ArXiv 的 top-1/top-3 仍为 71.60/93.76，接近未攻击的 74.84/95.76。provider 从 10 增至 100 时 top-1 会下降，但 WASA 仍高于 BM25；在更大 provider 数量时 top-k 更实用。生成质量几乎保持：原 GPT2 perplexity 12.4682，WASA-LLM 12.6570，distinct-1/2 几乎相同，但 GPT-4 评估的 naturalness 下降更明显，可能来自不可见 Unicode 水印。

## Ablation / Error Analysis
消融覆盖专用 watermark embedding space、文本/水印预测空间分离、TF-IDF 句子选择和 enforced watermark generation。TF-IDF 选代表句比随机选句在 attribution accuracy 与生成质量之间更好；分离空间和专门 watermark token 能减少错误水印。错误大多是 misclassification，即生成的水印属于另一个 provider，而不是完全无效的 unknown watermark，原因通常是多个 provider 的写作特征重叠。

攻击强度提高会降低 accuracy，例如 20% insertion/deletion/synonym attack 后生成文本的 top-1 仍约 65.12/70.00/69.20；输入文本攻击也有下降但 top-k 保持较高。局限性实验还显示，若真实来源是未水印的公共预训练数据，系统只能把它视为不可归属来源。

## Limitations
论文假设 provider 有足够且相对平衡、风格可区分的数据，并主要研究单一主来源；真正混合来源、短文本、未水印预训练语料和更强的改写攻击更困难。不可见 Unicode 可能被清洗，naturalness 有下降；作者也明确指出尚不清楚对 adversarial training 等更高级攻击的鲁棒性。

## 两句话总结
WASA 把 data provider 编码为不可见水印，让 LLM 在生成文本时同时输出可解码的来源标记，从而把 IP provenance 变成可查询的 source attribution。它相对 BM25 在多个数据集和模型上大幅提高归因准确率，并能从被篡改文本重建水印，但依赖可区分的 provider 风格和未被强力改写的文本。

## Evidence note
已逐段读取本地 PDF，核对了水印设计、TF-IDF top-20%、两阶段 WASA-LLM、BM25 对照、Table 1/2/3/4 数值、攻击/扩展/质量实验和错误类型。
