# 108. Does Fine-Tuning LLMs on New Knowledge Encourage Hallucinations?

> 人工精读笔记：EMNLP 2024。重点是 Known/Unknown factual examples 的区分、学习速度差异和 fine-tuning 后 hallucination 的来源。

## 0. 论文信息

- **作者**：Zorik Gekhman, Gal Yona, Roee Aharoni, Mata 等
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.444)
- **主题**：new knowledge fine-tuning、factual recall、hallucination、knowledge integration

## 1. Introduction：要解决什么问题

LLM 经常通过 supervised fine-tuning 学习新事实，但新知识与预训练 knowledge 的组合可能产生副作用：模型一方面要记住训练数据，另一方面又要在旧知识和新知识之间保持一致。一个直觉猜想是，fine-tuning 新知识会让模型更容易 hallucinate；论文用控制实验检查这个猜想，而不是只比较 fine-tuning 前后总体 factuality。

关键是区分 Known facts（模型预训练前已经知道）与 Unknown facts（需要从 fine-tuning examples 新学）。如果把两者混在一起，无法知道 hallucination 是因为新知识本身，还是因为训练样本难度/分布不同。

## 2. Method / Framework：怎么解决

作者构建 QA fine-tuning 数据，按模型对事实的 pre-training knowledge 将样本分成 Known、Unknown 等类别，并控制问题格式、答案长度和其他因素。通过改变 Unknown examples 在 fine-tuning data 中的比例，测量模型学习速度、旧知识保留和 hallucination。

实验记录每个训练 checkpoint 的 answer correctness、generalization 和 hallucinated response；同时比较去掉 Unknown examples 的训练，观察是否能降低 overfitting 而不牺牲新知识性能。

## 3. Baseline / 对比基线

- **Original pretrained LLM**：没有新知识 fine-tuning 的基线。
- **Known-only fine-tuning**：只学习模型已有事实。
- **Unknown-only / mixed fine-tuning**：隔离新知识组成。
- **不同 Unknown proportion**：测试新知识数量与 hallucination 的关系。
- **训练 duration/checkpoint**：区分早期学习、过拟合和长期行为。

## 4. Experiments / Findings：结果如何读

Unknown examples 学得明显比 Known examples 慢，说明 fine-tuning 不像 pre-training 那样容易把全新事实整合进参数。随着模型逐渐学会 Unknown facts，hallucination 风险会增加，支持“新知识 integration 与副作用相关”的观察；但风险并不是简单由训练样本数量线性决定。

Known examples 的学习通常更快，fine-tuning 可能强化已有 parametric knowledge；Unknown examples 更容易被表面模式拟合，尤其在训练时间较长时出现过拟合/错误泛化。移除 Unknown examples 能显著降低部分 overfitting/hallucination，同时不必然牺牲 Known knowledge performance。

论文还发现三类 Known knowledge 的组成和难度不同，fine-tuning examples 的 composition 会改变结果。因此“fine-tuning new knowledge causes hallucination”不能简单解释为训练方式本身，知识是否原本存在、事实组合和训练时间同样关键。

## 5. Ablation / 机制解释

- Known/Unknown 标签消融，检验新知识而非格式差异。
- Unknown ratio sweep，量化新知识比例。
- 去掉 Unknown examples，测试减轻 overfit 的措施。
- 多 checkpoint，观察 learning curve 与 hallucination 的时间关系。
- 控制答案/问题形式，排除模板和长度混杂。

## 6. Limitations / 局限与复现注意

- Known/Unknown 依赖模型探测，模型“没答对”不等于参数中完全没有知识。
- QA 数据与事实类型有限，不能直接推广到开放生成、对话或多跳知识。
- hallucination 评价可能依赖自动 exact match/外部判断，需核对语义等价。
- 论文研究 fine-tuning 训练轨迹，不提供一个通用 anti-hallucination 算法。

## 7. 两句话总结

论文通过 Known/Unknown factual examples 的控制实验发现，LLM 学新知识明显慢于重学已有知识，并且真正整合新知识时可能增加 hallucination/过拟合风险。减少 Unknown fine-tuning examples 可降低部分风险但也会减少新知识覆盖，说明数据组成和训练时间决定了这场 trade-off。
