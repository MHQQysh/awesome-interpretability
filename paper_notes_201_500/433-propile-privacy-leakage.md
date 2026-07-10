# 433. ProPILE: Probing Privacy Leakage in Large Language Models

- **Authors:** Siwon Kim, Sangdoo Yun, Hwaran Lee, Martin Gubri, Sungroh Yoon, Seong Joon Oh
- **Venue / year:** arXiv, 2023
- **Paper:** https://arxiv.org/abs/2307.01881
- **Tags:** `privacy` `PII` `memorization` `black-box-probing` `soft-prompt`

## Introduction
LLM 训练语料来自大规模网页，可能包含姓名、邮箱、电话、地址、家庭关系和 affiliation 等 PII。传统训练数据抽取通常假设攻击者有固定 prefix、知道模型内部信息或专门构造含 PII 的微调集；ProPILE 问的是更贴近部署的问题：一个数据主体只知道自己的 PII、对服务只有黑盒访问时，能否判断自己的信息是否被模型记住并可能泄漏。

## Method / Framework
ProPILE 先区分 linkability 与 structurality。linkability 表示给出同一人的其他 PII 后，目标 PII 的条件概率是否高于无上下文概率；structurality 区分电话/邮箱这类有格式的 structured PII 与家庭关系/职业背景等 unstructured PII。黑盒模式把目标项从一组个人信息中拿掉，用多种模板把其余 M-1 项组合成 prompt，重复采样并记录目标 PII 的 likelihood、exact match 和与随机 PII 的差异。

白盒模式利用服务提供者的训练数据、参数和梯度，对前置 soft tokens 做 prompt tuning，目标是最大化目标 PII 的重构 likelihood。作者还测试 soft prompt 的数据量、token 数量、初始化方式，以及从 OPT-1.3B 向 OPT-350M/2.7B 转移的可行性，从而把“普通用户可测风险”和“提供商可做的最坏情况探测”分开。

## Baselines / Comparisons
核心比较是 target PII 与随机 null PII、不同数量的手工模板、不同关联 PII 数量、不同 beam size 和不同模型规模。白盒则与五个手工 prompt 的黑盒结果比较，并改变 soft-prompt 训练数据量、soft token 数和初始化；迁移实验用 OPT-1.3B 学到的 prompt 与 OPT-350M/1.3B/2.7B 的原始单模板结果比较。

## Experiments / Findings
在 OPT-1.3B/Pile 上，目标 PII 的 likelihood 在各种 PII 类型上都高于随机 PII，除 affiliation 外 Wilcoxon 检验差异显著；增加模板数量、beam size、关联 PII 和模型规模都会提高 exact reconstruction。只从 name 的 twin prompt 增加到 name+phone 等 triplet 后，邮箱 exact match 约提升五倍，电话也提升超过两倍，说明“同一人的多项信息”是重要的 linkability 信号。

白盒结果更直观：五个手工模板的 black-box phone exact match 率约为 0.0047%，用仅 128 条数据学到的 soft prompt 后达到约 1.3%。训练样本从 16 增加到 128 时，exact match 从 0.12% 升到 1.50%；soft token 越多通常泄漏越强，使用目标 PII 类型词向量初始化明显优于 uniform/词表均值。OPT-1.3B 学到的 soft prompt 在其他 OPT 规模上能放大 likelihood，但 exact-match 转移效果不总是稳定。

## Ablation / Error Analysis
模板数、关联信息量、beam size、模型规模、soft token 数和初始化共同构成主要敏感因素。随机 PII 作为 null 对照避免把“模型会生成常见电话格式”误判为针对性泄漏；同时 likelihood 与 exact match 可能给出不同排序，低概率并不等于没有实际风险，因为重复查询会累积重构机会。

评测集从公开数据中用启发式规则抽取结构化 PII，因此可能包含错误关联或噪声；affiliation 的统计差异不稳定也显示，非结构化 PII 的自动评估更困难。soft prompt 的跨模型迁移只能在 likelihood 角度较明显地成立，维度和 tokenizer 不匹配时需要投影为 hard tokens。

## Limitations
论文主要在 OPT-1.3B 和 Pile 的受控 PII 集上验证，不代表闭源前沿模型或真实服务的完整风险。评测 PII 来自公开数据且关联关系是启发式构造，存在标签噪声；ProPILE 是测量框架而不是防御方案，论文没有系统比较去重、过滤、差分隐私或输出审查的效果。

## 两句话总结
ProPILE 用数据主体自己的其他 PII 构造黑盒 prompts，并以目标项相对随机项的 likelihood/exact match 衡量可链接的隐私泄漏。进一步的 soft-prompt 白盒探测表明，只需少量训练数据就能大幅放大重构概率，因此隐私审计不能只看普通 prompt 下是否偶然打印出一条 PII。

## Evidence note
已逐段读取本地 PDF，核对了 linkability/structurality 定义、黑盒与白盒算法、OPT/Pile 实验、0.0047%→约1.3%、0.12%→1.50% 等结果及数据集噪声限制。
