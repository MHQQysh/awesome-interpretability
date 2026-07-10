# 106. STORYSUMM: Evaluating Faithfulness in Story Summarization

> 人工精读笔记：EMNLP 2024。重点是故事摘要中的局部事实错误、人工标注协议，以及自动 faithfulness metric 的失效。

## 0. 论文信息

- **作者**：Melanie Subbiah, Faisal Ladhak 等
- **来源**：[EMNLP 2024](https://aclanthology.org/2024.emnlp-main.557)
- **资源**：STORYSUMM；Reddit short stories 与多模型生成摘要
- **任务**：逐句 faithfulness 标注、自动评估模型对人工判断的 agreement

## 1. Introduction：要解决什么问题

故事摘要比新闻摘要更开放，人物、事件、时序和隐含信息复杂；BLEU/ROUGE、readability 和 coherence 不能判断摘要是否忠实于原故事。一个摘要可能文笔流畅、整体主题正确，却在一个句子中加入故事没有的事实。

作者构建 STORYSUMM，给短故事摘要提供 localized sentence-level faithfulness labels 和解释，并比较普通 annotator、专家 adjudication 与 GPT/LLM judge。目标是为 narrative summarization 提供比粗粒度二元标签更可靠的评测资源。

## 2. Method / Framework：怎么解决

从 Reddit short stories 生成摘要，按摘要句子判断每句是否被源故事支持。标注员回答句子是否 faithful，并说明不一致；专家复核争议案例，构造 expanded gold labels。论文特别区分真正的事实错误、合理推断和故事开放性导致的主观歧义。

数据划分 validation/test，报告 annotator agreement、Cohen kappa 以及自动模型与 gold 的一致性。fine-grained protocol 让评价者能指出局部错误，而不是把整篇摘要强行判为 faithful/unfaithful。

## 3. Baseline / 对比基线

- **Upwork annotators**：普通 crowdsourcing 标注。
- **Expert labels / hybrid labels**：专家校准后的 gold。
- **GPT-4、Mixtral、其他 LLM judges**：自动 binary faithfulness evaluator。
- **FABLES、MiniCheck 等自动 factuality methods**：句子/摘要级 faithfulness baseline。
- **Readability/coherence metric**：展示“写得好”与“事实忠实”不是同一个目标。

## 4. Experiments / Findings：结果如何读

三位专家对一些故事摘要最初只部分一致，说明 narrative faithfulness 比新闻事实核验更模糊。Upwork annotators 可能漏掉 hard inconsistencies，专家/混合标注会发现新增错误；因此论文建议在高 precision 场景使用精细标注和专家 adjudication。

三类 LLM 常在同一摘要上犯相似错误，特别是把合理但未明示的推断当作原文事实。自动评估与普通 annotator 的 agreement 会被标签噪声夸大或低估；expanded gold 后，FABLES with GPT-4 总体表现最好，但不同方法的 agreement 仍有限。

论文报告不同自动方法分数差异很大，MiniCheck 的 agreement 可低至约 18%，Mixtral judge 可达约 97% 的某些设置，说明数字高度依赖模型、prompt 和 gold 版本，不能简单宣布某个 metric 普适。

## 5. Ablation / 机制解释

- annotator-only vs expert-expanded gold：衡量标注噪声影响。
- binary sentence labels vs fine-grained explanation：检验局部错误定位的价值。
- GPT-4/Mixtral/MiniCheck/FABLES：比较 LLM judge、NLI/entailment 和专用 faithfulness 模型。
- validation/test 及不同 prompt：检查自动 evaluator 的稳定性。

## 6. Limitations / 局限与复现注意

- Reddit 故事具有来源偏差，不能代表新闻、长篇小说或多语言叙事。
- faithful/unfaithful 边界需要语境和专家判断，自动 gold 仍可能含有争议。
- 模型污染、故事重叠和生成摘要 prompt 会影响评测。
- STORYSUMM 主要评价摘要句子是否忠实，不覆盖摘要是否全面、文学质量或长期人物一致性。

## 7. 两句话总结

STORYSUMM 解决故事摘要中“流畅但局部编造”难以被传统摘要指标发现的问题，提供句子级 faithfulness 标签和专家扩展 gold。实验显示普通标注和自动 judge 对隐含推断/难事实不一致并不稳定，FABLES 等方法有优势但强依赖标注协议和模型选择。
