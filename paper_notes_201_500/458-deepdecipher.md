# 458. DeepDecipher: Accessing and Investigating Neuron Activation in Large Language Models

- **Authors:** Albert Garde, Esben Kran, Fazl Barez
- **Venue / year:** XAI in Action @ NeurIPS 2023
- **Paper:** [arXiv:2310.01870](https://arxiv.org/abs/2310.01870)
- **Tags:** `neuron-interpretability` `tooling` `MLP` `mechanistic-interpretability`

## Introduction
attention 得到很多 interpretability 工具，而 Transformer MLP neuron 的激活、触发上下文和概念关系仍不容易访问。DeepDecipher 不提出新的解释算法，而是把已有的 Neuron2Graph、Neuroscope 和 GPT-4 Neuron Explainer 数据整合成 API 与网页，降低研究者检查模型内部的门槛。

## Method / Framework
Neuron2Graph 从高激活 token 回溯并替换上下文，构造影响 neuron 激活的 token dependency graph；Neuroscope 为每个 neuron 提供 20 条最强激活的 1024-token 片段；Neuron Explainer 用 GPT-4 生成概念描述并评估描述对激活的预测能力。系统通过 `/api/model/service/layer/neuron` 提供 JSON，网页按模型、layer、neuron 展示图、样本和元数据。

## Baselines / Comparisons
与 Neuroscope 网站和 OpenAI Neuron Explainer 比较数据覆盖、可用模型、输出格式和请求速度。DeepDecipher 支持 25 个模型、可扩展三种服务；表中报告 API/web page 速度约 56/87/53 requests per second，但作者特别说明这些主要受客户端下载速度限制。

## Experiments / Findings
工具案例显示，solu-6l-pile 中 `hello` 有 7 个响应 neuron，而 `hola` 为 0，与英语过滤数据一致；情绪词响应数为 happy 9、sad 5、hate 3、love 20、morose 0；对 `apple` 的 8 个响应 neuron 中 5 个偏公司、2 个偏水果、1 个含糊，展示了同形词概念分化。系统还支持相似 neuron 搜索和局部部署，便于快速提出/检查假设。

## Ablation / Error Analysis
本文没有模型准确率 ablation；比较的是工具访问效率和服务覆盖。作者明确指出最高激活样本未必代表 neuron 的全部功能，GPT-4 描述可能只是近似标签，且“用户能否凭网页形成可靠机制理解”还没有做系统用户实验。

## Limitations
Neuron2Graph 当时只对单一模型可用，Neuron Explainer 仅覆盖 GPT-2-small/XL；数据来自固定语料和预计算激活，不能自动说明 neuron 对最终行为的因果贡献。工具本身是 interpretability infrastructure，不应把 token 共现图误读为机制证明。

## 两句话总结
DeepDecipher 把 MLP neuron 的 dependency graph、top-activating text 和自动概念说明整合成可查询 API/网页，使低层解释更容易复用。它的价值在于可访问性和规模化，而不是新解释算法，因果性、代表性和用户理解效果仍需额外验证。

## Evidence note
已读取服务定义、API/网站设计、25 模型对比、案例统计、速度表和作者对数据代表性/因果性的限制说明。
