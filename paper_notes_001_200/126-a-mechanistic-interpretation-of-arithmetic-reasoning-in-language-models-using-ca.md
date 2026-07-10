# 126. A Mechanistic Interpretation of Arithmetic Reasoning in Language Models using Causal Mediation Analysis

> 人工精读笔记：EMNLP 2023。重点是用 causal mediation 追踪 operand/operator -> attention transfer -> late MLP result generation。

## 0. 论文信息

- **作者**：Alessandro Stolfo, Yonatan Belinkov, Mrinmaya Sachan
- **来源**：[EMNLP 2023](https://aclanthology.org/2023.emnlp-main.435)
- **模型**：GPT-J、Pythia、Llama、Goat 等
- **任务**：二/三操作数加减乘除与非算术 number retrieval/factual control

## 1. Introduction：要解决什么问题

LLM 能回答简单算术，但内部是否真正计算、哪些组件传递 operands 和 result information 并不清楚。论文用因果干预代替只看 activation，试图定位对最终数字概率有直接影响的中间模块。

## 2. Method / Framework：怎么解决

把模型视为 input-to-output causal graph，对 attention/MLP activation 做 clean/corrupted/patched intervention，计算 indirect effect：替换某组件后正确结果相对错误结果的 log probability 变化。固定 operands 或固定 result 的成对 query 用来分离 operand-related 与 result-related information。

## 3. Baseline / 对比基线

不同 layer 的 MLP/attention、result-varying vs result-fixed query、二/三 operand、未 fine-tune vs arithmetic fine-tuned Pythia、number retrieval 和 factual knowledge task。

## 4. Experiments / Findings：结果如何读

早层/中层处理 operands 和 operator，attention 把相关信息传到序列末 token；中后层 MLP 将 result-related information 写入 residual stream并影响最终数字。Pythia 2.8B fine-tuning 后，约 layer 19/20 的 MLP 出现更强 result incorporation；未 fine-tune 模型的路径不同。

与 number retrieval/factual task 对照显示，算术任务的 mid-late MLP effect 更集中，说明它不是普通数字复制。阿拉伯数字和英文数字模板有约 50% top neuron overlap，提示存在共享数字处理子结构。

## 5. Ablation / 机制解释

固定 result、固定 operands、不同 operator、三操作数、模型/任务对照，以及 log probability difference vs alternative importance metric 构成消融。结果支持三阶段：A operand/operator processing，B attention information transfer，C late MLP result generation。

## 6. Limitations / 局限与复现注意

任务是小规模模板化 arithmetic，不能代表复杂数学证明；研究以完整 attention/MLP block 为单位，未定位单 head/neuron；tokenization、结果范围和干预强度会影响间接效应。

## 7. 两句话总结

论文用 causal mediation 发现算术信息先在中序列早层处理，再由 attention 传到末 token，最后由晚层 MLP 注入 result-related logits。与数字检索和事实任务的对照支持算术特有路径，但模板化任务和 block-level attribution 限制了推广。
