# 372. Vision-Language Interpreter for Robot Task Planning

- **Authors:** Keisuke Shirai, Cristian C. Beltran-Hernandez, Masashi Hamaya, Atsushi Hashimoto, Shohei Tanaka, Kento Kawaharazuka, Kazutoshi Tanaka, Yoshitaka Ushiku, Shinsuke Mori
- **Venue / year:** ICRA, 2024
- **Paper:** https://arxiv.org/abs/2311.00967
- **Tags:** `VLM` `LLM` `robot-planning` `PDDL` `symbolic-verification` `interpretability`

## Introduction
语言指导的机器人系统需要把自然语言指令和视觉场景转成可执行动作。端到端语言模型可以生成灵活的计划，但内部决策难验证；符号规划器则能给出机器可检查的动作序列，却要求输入是严格的 PDDL problem description (PD)。本文把两者连接起来，研究一个更具体且可验证的问题：给定 scene observation 和 linguistic instruction，能否生成正确的 PD，让 symbolic planner 找到有效计划？

论文将失败拆成三种来源：对象列表 O 错了、初始状态 I 错了、目标条件 G 错了。这样比只报告“机器人成功/失败”更可诊断，也让生成模型的中间产物变成人类和规划器都能检查的解释接口。

## Method / Framework
ViLaIn 是一个由 VLM、LLM 和 symbolic planner 组成的管线：

1. 用 open-vocabulary object detector 得到场景对象；用 BLIP-2 对对象裁剪图生成 caption，再由 GPT-4 把对象和属性整理成 PDDL objects O 与 initial state I。
2. 将语言指令、O、I、domain knowledge 和同域的输入/输出例子交给 GPT-4，由它生成目标 specification G。
3. 将 O、I、G 组合成 problem.pddl，交给 Fast Downward 规划器。规划器返回有效计划，或返回 grammar/semantic error message。
4. 对失败 PD 进行 corrective re-prompting (CR)：把错误信息交回 LLM，让它只修正相关部分。生成时还比较 chain-of-thought (CoT) 与一次生成。

作者构建 ProDG 数据集，覆盖 cooking、Blocksworld、Hanoi 三个域。每个域有一个 domain description 和十个 PD；每个问题有语言指令和场景观测。Hanoi 特别考验模型能否根据对象和状态生成不同目标，而不是只复述固定文本。

## Baselines / Comparisons
这不是传统的“训练一个模型对比另一个模型”的论文，比较重点是生成配置：GPT-4 few-shot 生成、是否使用 CoT、是否使用 CR、以及一次生成完整 PD 的 ViLaIn-whole。Fast Downward 用于规划，VAL 用于检查 PD 的语法和计划有效性。作者还将工作放在已有语言到机器人动作、PDDL 场景描述、文本到运动规划方法的脉络中，但没有用一个端到端机器人策略作为同等指标的主 baseline。

评价指标分别对应四层正确性：Rsyntax 检查 VAL 是否报语法警告；Rplan 检查是否存在 VAL 接受的有效计划；Rpart 分别计算 O、I、G 对 ground truth 的召回；Rall 要求一个问题包含所有 ground-truth 对象和命题，是更严格的整体语义指标。

## Experiments / Findings
ProDG 主结果如下：Cooking 的 Rsyntax/Rplan 为 0.99/0.99，Rpart(O/I/G) 为 1.00/0.93/0.93，Rall 为 0.71；Blocksworld 为 0.99/0.94，Rpart 为 0.98/0.79/0.89，Rall 为 0.36；Hanoi 为 1.00/0.58，Rpart 为 0.89/0.46/0.33，Rall 为 0.12。由此可以区分“能写出合法 PDDL”和“写出了目标任务”：语法几乎全部正确，但 Hanoi 的长时程约束和隐含对象关系让整体语义正确率明显下降。

生成完整 PD 的一次性配置在 Cooking 上 Rsyntax/Rplan/Rall 为 1.00/1.00/0.54，在 Blocksworld 上为 0.99/0.99/0.13，在 Hanoi 上为 1.00/0.94/0.21。它提高了部分计划指标，却降低了 Rall，说明一次生成更容易形成一个内部自洽但漏掉 ground-truth 命题的任务。

## Ablation / Error Analysis
配置消融表显示，Cooking 中 CoT+两次 CR 的 Rsyntax/Rplan/Rall 为 0.99/0.99/0.71；一次 CR+CoT 为 0.99/0.94/0.68；一次 CR 不带 CoT 为 0.97/0.85/0.59；不带 CR 和 CoT 只有 0.60/0.18/0.09。说明 planner feedback 是主要增益来源，CoT 在修复复杂状态时进一步帮助模型保持 O、I、G 的一致性。

错误分析指出，模型常漏掉 Hanoi 的 proposition，或把视觉检测/对象 caption 的错误传播到 I 和 G；Blocksworld 还会产生自相矛盾的 `on` 关系。CR 能修正语法和一部分可由 planner 发现的语义错误，但 planner 看不到“这个任务是否真的是用户想要的任务”，因此 Rall 仍显著低于 Rsyntax/Rplan。这是一个很重要的解释性边界：可验证的计划不等于正确理解。

## Limitations
数据集每个域规模较小，域描述和问题主要由作者构造，尚不足以代表开放世界机器人。VLM 的对象识别和 caption 误差会被后续 LLM 放大；CR 依赖 planner 能产生有用错误信息，对不可检测的遗漏无能为力。实验主体依赖 GPT-4，成本、版本漂移和闭源性限制了复现，也没有报告真实机器人执行中的感知、抓取和控制误差。

## 两句话总结
ViLaIn 将视觉场景和语言指令拆成 PDDL 的对象、初始状态与目标，再利用 symbolic planner 的错误信息迭代修正，从而把 LLM 计划变成可语法检查、可验证的中间表示。实验表明语法正确率超过 99%，但 Hanoi 的整体语义正确率只有 12% 的 Rall，说明形式验证能解释和修复一部分错误，却不能替代任务意图理解。

## Evidence note
已读取 arXiv 2311.00967 v2 本地 PDF 文本；表格数字来自 ProDG 主结果、whole-PD 对比和 CR/CoT 消融。
