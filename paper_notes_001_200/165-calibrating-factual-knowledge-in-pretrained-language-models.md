# 165. Calibrating Factual Knowledge in Pretrained Language Models

- **Authors:** Qingxiu Dong, Damai Dai, Yifan Song, Jingjing Xu, Zhifang Sui, Lei Li
- **Venue:** EMNLP Findings 2022
- **Paper:** https://aclanthology.org/2022.findings-emnlp.438
- **Tags:** Knowledge Editing, Factuality, Calibration, FFN

## Introduction

PLM 能存储事实，但事实可能错误，例如把 Sri Lanka 的首都预测成 Kingston。检索增强和任务特定知识编辑不能普遍修正模型内部知识；从头重训又成本高。论文提出 task-agnostic、轻量的 factual knowledge calibration。

## Method / Framework

先用 Contrastive Knowledge Assessing（CKA）比较正确 fact 与 fake fact 的分数，判断模型是否把正确知识排在更高位置。对错误事实冻结原模型参数，在对应 FFN 旁加入 calibration FFN / memory slots，只训练新增参数，并用 paraphrased factual text 让修正跨表面形式泛化。

评测包括 knowledge probing、closed-book QA 和 text generation，并可视化 calibration memory 对原始 FFN 的影响。

## Baselines / Comparisons

与原始 PLM、retrieval / fine-tuning、全参数知识编辑、其它 model editing 方法和只在一个句式上修正的 baseline 比较。指标包括 factual accuracy、wrong-fact suppression、paraphrase generalization、QA / generation 性能和参数 / 时间成本。

## Experiments / Findings

- CKA 能筛出模型在 right-vs-fake comparison 中排序错误的事实，避免对已经正确的知识重复编辑。
- CaliNet 只训练新增 calibration parameters，就能提高 probing factual accuracy，并在多个 text surface 上保持修正。
- closed-book QA 显示修正后的知识能泛化到问句和生成句式，而不是只记住校准模板。
- 参数量和计算开销显著低于全模型重训；原始知识和通用能力整体保持较好。

## Ablation / Error Analysis

消融 CKA selection、calibration slots 数量、paraphrase augmentation、插入 FFN 层和是否冻结原参数。只用单一 surface 容易过拟合；slots 太少无法表达多个冲突事实，太多会影响通用预测。多跳关系、事实相互依赖和事实本身歧义是主要错误来源。

## Limitations

CKA 只检测给定 probing set 中的错误，不能发现所有错误或隐性事实。FFN 插入位置依赖模型架构，编辑不保证删除旧知识，也可能在未测上下文中发生冲突。实验多是短事实与英文 PLM，不能直接推出开放世界和闭源 LLM 的安全校准。

## 两句话总结

CaliNet 先用对比 probing 找到模型排序错误的事实，再冻结原模型、只增加 FFN calibration memory 来修正并泛化到 paraphrase。它提供了比全量重训更轻量的事实编辑路径，但检测覆盖、旧知识残留和知识冲突仍需外部验证。
