# 101. FAITHSCORE: Fine-grained Evaluations of Hallucinations in Large Vision-Language Models

> 人工精读笔记：Findings of EMNLP 2024。重点是把自由形式 LVLM 答案拆成可验证的 atomic image facts，而不是只数对象是否出现。

## 0. 论文信息

- **作者**：Liqiang Jing, Ruosen Li, Yunmo Chen, Xinya Du
- **来源**：[Findings of EMNLP 2024](https://aclanthology.org/2024.findings-emnlp.290)
- **代码**：[FAITHSCORE](https://github.com/bcdnlp/FAITHSCORE)
- **数据/模型**：LLaVA-1k、MSCOCO-Cap；LLaVA、MiniGPT-4 等 LVLM

## 1. Introduction：要解决什么问题

LVLM 的自由形式回答可能同时包含客观图像描述和主观分析；现有 CHAIR、POPE、NOPE 等方法多关注对象是否存在，无法细粒度处理颜色、数量、空间关系和动作，也不适合开放式 VQA/对话答案。若把“合理分析”误判为 hallucination，或忽略一句话中的一个错误事实，分数都会失真。

FAITHSCORE 的目标是 reference-free、fine-grained 地衡量“答案是否忠实于图像”，并让每一个被判为错误的 atomic fact 都可解释地定位到答案片段。

## 2. Method / Framework：怎么解决

1. **Descriptive sub-sentence identification**：用 LLM 找出需要图像核验的客观描述句，排除纯主观/常识分析。
2. **Atomic fact generation**：把句子拆成对象、属性、数量、动作和关系等最小 facts。
3. **Visual entailment verification**：用视觉 entailment model 判断每个 atomic fact 是否被输入图像支持。

FaithScore 是被验证 facts 的比例，因此一句话“man is standing on the back of a car”与图像不一致时只惩罚这个关系，而不是把整段回答二元判错。

## 3. Baseline / 对比基线

- **CHAIR**：对象 hallucination 的 sentence/instance 指标。
- **POPE/NOPE**：通过 object probing 或负对象问题检查不存在对象。
- **CLIPScore、BLEU/ROUGE/METEOR**：相似度/重叠指标，不直接核验事实。
- **人工 Likert 评分**：作为 faithfulness gold，测试自动指标与人类判断的相关性。
- **不同 recognizer/verifier LLM 与 VEM**：消融子句识别器、事实生成器和 visual entailment model 的选择。

## 4. Experiments / Findings：结果如何读

在人类标注的 LVLM hallucination 上，FAITHSCORE 与人类判断的相关性最高或接近最高，优于单纯文本重叠和粗粒度对象指标。附录相关性表显示，BLEU/ROUGE/METEOR 对 hallucination 的相关性弱，CHAIR 只看对象时会漏掉关系/属性错误；FaithScore 的 atomic verification 更贴近人类判断。

在 LLaVA-1k 和 MSCOCO-Cap 上，LLaVA、LLaVA-1.5、InstructBLIP、MiniGPT-4 等模型都暴露出明显视觉 hallucination。captioning 相对短、事实少时更忠实；开放式复杂问题和更长回答更容易引入额外属性、空间关系和分析性错误。

闭源 GPT-4V/Gemini 的 FAITHSCORE 通常优于开源模型，但并非没有 hallucination。论文用句子级分数和答案长度分析说明，不能只报告整体均值：生成更长的答案会覆盖更多事实，同时也增大出错机会。

## 5. Ablation / 机制解释

- 只核验 object existence vs 解析 attributes/relations：后者与人类判断相关性更高。
- 不同 descriptive sentence recognizer：ChatGPT 通常优于较小 LLaMA recognizer。
- 不同 visual entailment model：VEM 的质量直接决定 atomic fact 标签。
- answer-level vs sentence-level score：sentence-level 能定位错误来源，适合诊断模型。

## 6. Limitations / 局限与复现注意

- 依赖 LLM 进行句子识别和 atomic fact generation，前置解析错误会传递到最终分数。
- VEM 仍可能无法判断复杂常识、隐含关系和主观分析，reference-free 不等于无偏。
- 人类 gold 规模有限，跨语言、视频和多轮视觉对话还需额外验证。
- FaithScore 是事实忠实性指标，不评价答案有用性、推理质量或安全性。

## 7. 两句话总结

FAITHSCORE 解决 LVLM 自由回答中细粒度视觉 hallucination 无法被 CHAIR/POPE 等粗粒度指标覆盖的问题。它把回答拆成描述句和 atomic image facts，再用视觉蕴含验证，结果比文本相似度和对象级指标更贴近人类判断，但解析器和 VEM 的误差仍会影响最终分数。
